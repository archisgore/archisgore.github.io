---
layout: post
title: "How we built software at Polyverse"
description: "This is an Engineering post on how we build software at Polyverse, what processes we follow and why we follow them."
date: 2017-08-24 23:42:24 +0000
original_url: https://medium.com/@archisgore/how-we-build-software-at-polyverse-49bbd86d817
---

This is an Engineering post on how we build software at Polyverse, what processes we follow and why we follow them.

A couple of weeks ago, I attended a [CoffeeOps](https://www.meetup.com/Seattle-CoffeeOps/) meetup at [Chef](https://www.chef.io/) HQ. One of my answers detailing how we do agile, CI/CD, etc. got people excited. That prompted me to describe in detail exactly how our code is built, shipped, and how we simplified many of the challenges we saw in other processes. It should be no surprise that we make heavy use of [Docker](https://www.docker.com/) for getting cheap, reliable, and consistent environments.

### Dropping Irrelevant Assumptions

I first want to take a quick moment to explain how I try to approach any new technology, methodology or solution, so that I make the best use of it.

Four years ago, when we brought Git into an org, a very experienced and extremely capable engineer raised their hand and asked, “I’ve heard that Git doesn’t have the feature to revert a single file back in history. Is this true? If it is true, then I want to understand why we are going backwards.”

I will never forget that moment. As a technical person, truthfully, that person was absolutely RIGHT! However, moving a single file backwards was something we did because we didn’t have the ability to cheaply tag the “state” of a repo, so we built up terrible habits such as “branch freeze”, timestamp-based checkouts, “gauntlets”, etc. It was one of the most difficult questions to answer, without turning them antagonistic, and without sounding like you’re evading the issue.

I previously wrote a similar answer on Quora about Docker and [why the worst thing you can do is to compare containers to VMs](https://www.quora.com/What-are-some-disadvantages-of-using-Docker/answer/Archis-Gore?__filter__&__nsrc__=2&__snid3__=1321475629).

It is very dangerous to stick to old workarounds when a paradigm shift occurs. Can we finally stop it with [object pools for trivial objects in Java](https://softwareengineering.stackexchange.com/questions/115163/is-object-pooling-a-deprecated-technique)?

### What problems were we trying to solve?

We had the same laundry list of problems nearly any organization of any type (big, small, startup, distributed, centralized, etc.) has:

1. First and foremost, we wanted to be agile, as an adjective and a verb, not the noun. We had to demonstrably move fast.
2. The most important capability we really wanted to get right was the ability to talk about a “thing” consistently across the team, with customers, with partners, with tools and everything else. After decades of experience, we’d all had difficulty in communicating precise “versions” of things. Does that version contain a specific patch? Does it have all the things you expect? Or does it have things you don’t expect?
3. We wanted to separate the various concerns of shipping and pay the technical debt at the right layer at all times: developer concerns, QA assertions, and release concerns. Traditional methods (even those used in “agile”, were too mingled for the modern tooling we had). For example, “Committing code” is precisely that — take some code and push it. “Good code” is a QA assertion — regardless of whether it be committed or not. “Good enough for customers” is a release concern. When you have powerful tools like Docker and Git, trying to mangle all three in some kind of “master” or “release” branch seemed medieval!
4. We wanted no “special” developers. Nobody is any more or less important than anyone else. All developers pay off all costs at the source. One person wouldn’t be unfairly paying for the tech debt of another person.
5. We believed that “code is the spec, and the spec is in your code”. If you want to know something, you should be able to consistently figure it out in one place — the code. This is the DRY principle (Don’t Repeat Yourself.) If code is unreadable, make it readable, accessible, factored, understandable. Don’t use documentation to cover it up.
6. We wanted the lowest cognitive load on the team. This means use familiar and de-facto standards wherever possible, even if it is occasionally at the cost of simplicity. We have a serious case of the “Not-Invented-Here” syndrome. :-) We first look at everything that’s “Not-Invented-Here”, and only if it doesn’t suit our purpose for a real and practical problem, do we sponsor building it in-house.

Now let’s look at how we actually build code.

### [Content-based](http://conver.io/) non-linear versioning

The very fundamental foundation of everything at Polyverse is content-based versioning. *A* [*content-based version*](http://conver.io/) *is a cryptographically secure hash over a set of bits, and that hash is used as the “version” for those bits.*

This is a premise you will find everywhere in our processes. If you want to tell your colleague what version of Router you want, you’d say something like: router@72e5e550d1835013832f64597cb1368b7155bd53. That is the version of the router you’re addressing. It is unambiguous, and you can go to the Git repository that holds our router, and get PRECISELY what your colleague is using by running `git checkout 72e5e550d1835013832f64597cb1368b7155bd53.`

This theme also carries over to our binaries. While there is semantic versioning in there, you’ll easily baffle anyone on the team if you asked them for “Router 1.0.2”. Not that it is difficult to look it up, but that number is a text string that anyone could place there and as a mental model, you’d make everyone a little uneasy. Culturally we simply aren’t accustomed to talking in imprecise terms like that. You’d be far more comfortable saying *Router with sha 5c0fd5d38f55b49565253c8d469beb9f3fcf9003*.

Philosophically we view Git repos as “commit-clouds”. The repos are just an amorphous cloud of various commit shas. Any and every commit is a “version”. You’ll note that this not only is an important way to talk about artifacts precisely, but more so, it truly separates “concerns”. There is no punishment for pushing arbitrary amounts of code to Git on arbitrary branches. There is no fear of rapidly branching and patching. There is no cognitive load for quickly working with a customer to deliver a rapid feature off of a different branch. It just takes away all the burden of having to figure out what version you assign to indicate “Last known good build of v1.2.3 with patches x, y, but not z”, and “Last known good build of v1.2.3 with patches x, z, but not y”.

Instead, anyone can look up your “version” and go through the content tree, as well as Git history and figure out precisely what is contained in there.

Right about now, I usually get pushback surrounding the questions: how do you know what is the latest? And how do you know where to merge?

That is precisely that “perforce vs git” mental break we benefit from. You see, versions don’t really work linearly. I’ve seen teams extremely frightened of reverting commits and terrified of removing breaking features rapidly. Remember that “later” does not necessarily mean “better” or “comprehensive”. If A comes later than B, it does not imply that A has more features than B, or that A is more stable than B, or that A is more useful than B. It simply means that somewhere in A’s history, is a commit node B. I fundamentally wanted to break this mental model of “later” in order to break the hierarchy in a team.

This came from two very real examples from my past:

1. I’ll never forget my first ever non-shadowed on-call rotation at Amazon. I called it the build-from-hell. For some reason, a breaking feature delayed our release by a couple of days. We were on a monthly release cycle. No big deal.  
   However, once the feature was broken, and because there was an implied but unspoken model of “later is better”, nobody wanted to use the word “revert”. In fact, it was a taboo word that would be taken very personally by any committer. At some point, other people started to pile on to the same build in order to take advantage of the release (because the next release was still a month away.) This soon degraded into a cycle where we delayed the release by about 3 weeks, and in frustration we pulled the trigger and deployed it, because reverting was not in the vocabulary — I might have just called the committer a bunch of profane names and an imbecile and a moron and would have been better off.
2. The event led to the very first Sev-1 of my life at 4am on one fateful morning. For the uninitiated, a Sev-1 is code for “holy shit something’s broken so bad, we’re not making any money off Amazon.com.”  
   After four hours of investigation it turned out that someone had made a breaking API change in a Java library, and had forgotten to bump up the version number — so when a dependent object tried to call into that library, the dispatch failed. Yes we blamed them and ostracized them, and added “process” around versioning, but that was my inflexion point — it’s so stupid!  
   What if that library had been a patch? What if it had 3 specific patches but not 2 others? Does looking at version “2.4.5-rc5” vs “2.4.5-rc4” tell you that rc5 has more patches? Less patches? Is more stable? Which one is preferable? Are these versions branch labels? Git tags? How you ensure someone didn’t reassign that tag by accident? Use signed tags? All of this is dumb and unnecessary! The tool gives you PRECISE content identifiers. It was fundamentally built for that! Why not use it? Back in SVN/CVS/Perforce, we had no ability to specifically point to a code snapshot and it was time we broke habits we picked up because of that. This is why Git does not allow reverting of a single one file. :-)

The key takeaway here was — these are not development concerns. We conflated release concerns with identity concerns. They are not the same. First, we need a way to identify and speak about the precise same thing. Then we can assert over that thing various attributes and qualities we want.

We didn’t want people to have undue process to make rapid changes. What’s wrong with making breaking API changes? Nothing at all! That’s how progress happens. Developers should be able to have a bunch of crazy ideas in progress at all times and commits should be cheap and easy! They should also have a quick, easy and reliable way of throwing their crazy ideas over the wall to someone else and say, “Hey can you check this version and see how it does?”, without having to go through a one-page naming-convention doc and updating metadata files. That was just so medieval!

What about dependency indirection? One reason people use symbolic references (like semantic versioning) is so that we can refer to “anything greater than 2.3.4” and not worry about the specific thing that’s used.

For one, do you REALLY ever deploy to production and allow late-binding? As [numerous incidents have demonstrated](https://www.theregister.co.uk/2016/03/23/npm_left_pad_chaos/), no sane Ops person would ever do this!

In my mind, having the ability to deterministically talk about something, far outweighs the minor inconvenience of having to publish dependency updates. I’ll describe how we handle dependencies in just a minute.

### Heavy reliance on Feature Detection

Non-linear [content-based versioning](http://conver.io/), clearly raises red-flags. Especially when you’re built around an actor-based model of microservices passing messages all over the place.

However, there’s been a solution staring us right in the face for the past decade. One that we learned from the web developers — use feature detection, not version detection!

When you have loosely-coupled microservices that have no strict API bindings, but rather pass messages to each other, the best way to determine if a service provides a feature you want, is to **just ask it**!

We found quite easily, that when you’re not building RPC-style systems, and I consider callbacks as still being an RPC-style system, you don’t even need feature-detection. If a service doesn’t know what to do with a message, it merely ignores it, and the feature simply doesn’t exist in the system. If you’re not waiting for a side-effect — not just syntactically, but even semantically, you end up with a very easy model.

Now that comment in the previous section about a developer being able to throw a version over the wall and ask another developer what they thought of it, makes a lot more sense. Someone can easily plug in a new version very easily into the system, and quickly assert whether it works with the other components and what features it enables.

This means that at any given time, all services can arbitrarily publish a bunch of newer features without affecting others for the most part. This is also what allows us to have half a dozen things in progress at all times, and we can quickly test whether something causes a regression, and whether something breaks a scenario. We can label that against a “version” and we know what versions don’t work.

Naturally this leads us to a very obvious conclusion, where “taking dependencies” areno longer required at a service/component level. They wouldn’t be loosely-coupled actor-model-based Erlang-inspired microservices, if they had dependency trees. What makes more sense is…

### Composition, not Dependencies

When you have content-based non-linear versioning allowing aggressive idea execution, combined with services that really aren’t all that concerned about what their message receivers do, and will simply log an error and drop weird messages sent to themselves, you end up with a rather easy solution to dependency management — composition.

If you’ve read my previous posts, or if you’ve seen some of our samples, you’ll have noticed a key configuration value that shows up all over the place called the **VFI**. It’s a JSON blob that looks something like this:

```
{  
  "etcd": {  
    "type": "dockerImage",  
    "address": "quay.io/coreos/etcd:v3.1.5"  
  },  
  "nsq": {  
    "type": "dockerImage",  
    "address": "nsqio/nsq:v0.3.8"  
  },  
  "polyverse": {  
    "type": "dockerImage",  
    "address": "polyverse-runtime.jfrog.io/polyverse:0e564bcc9d4c8f972fc02c1f5941cbf5be2cdb60"  
  },  
  "status": {  
    "type": "dockerImage",  
    "address": "polyverse-runtime.jfrog.io/status:01670250b9a6ee21a07355f3351e7182f55f7271"  
  },  
  "supervisor": {  
    "type": "dockerImage",  
    "address": "polyverse-runtime.jfrog.io/supervisor:0d58176ce3efa59e2d30b869ac88add4467da71f"  
  },  
  "router": {  
    "type": "dockerImage",  
    "address": "polyverse-runtime.jfrog.io/router:f0efcd118dca2a81229571a2dfa166ea144595a1"  
  },  
  "containerManager": {  
    "type": "dockerImage",  
    "address": "polyverse-runtime.jfrog.io/container-manager:b629e2e0238adfcc8bf7c8c36cff1637d769339d"  
  },  
  "eventService": {  
    "type": "dockerImage",  
    "address": "polyverse-runtime.jfrog.io/event-service:0bd8b2bb2292dbe6800ebc7d95bcdb7eb902e67d"  
  },  
  "api": {  
    "type": "dockerImage",  
    "address": "polyverse-runtime.jfrog.io/api:ed0062b413a4ede0e647d1a160ecfd3a8c476879"  
  }  
}
```

*NOTE: If you work at Amazon or have worked there before, you’ll recognize where the word came from. When we started at Polyverse, I really wanted a composition blob that described a set of components together, and I started calling it a VFI, and now it’s become a proper noun. It really has lost all meaning as an acronym. It’s simply its own thing at this point.*

What you’re seeing here, is a set of components that describe as you guessed it, the addresses where they might be obtained (in this example, the addresses are symbolic — they’re Docker image labels; however in highly-secure deployments we use the one true way to address something — content-based shas. You might easily see a VFI that has “`polyverse-runtime.jfrog.io/api@sha256:<somesha>`” in the address field.

Again, you’ll notice that this isn’t a fight against proper dependencies, but rather an acknowledgement that “router” is not where information for “all of polyverse working” should be captured. It is a proper separation of concerns.

The router is concerned with whether it builds, passes tests, boots up and has dependencies it requires for its own runtime. What doesn’t happen is a router taking dependencies at the component level, on what the container manager should be, could be, would be, etc. And more so, it does not have the burden of ensuring that “cycling works”.

Too often these dependency trees impose heavy burdens on developers of a single component. In the past I’ve seen situations where, if you’re a web-server builder, and you got a downstream broken dependency related to authentication, you are now somehow de-facto responsible for paying the price of the entire system’s end-to-end working. It means that the burden of work increases as you move further upstream closer to your customer. One bad actor downstream, has you paying the cost. Sure, we can reduce the cost by continually integrating faster, but unless “reverting” is an option on the table, you’re still the person who has to do it.

This is why Security teams are so derided by the Operations teams. Until recently and the advent of [DevSecOps](https://www.meetup.com/Seattle-DevSecOps/), they always added a downstream burden — they would publish a library that is “more secure” but breaks a fundamental API, and you as the developer, and occasionally the operator paid the price for updating all API calls, testing and verifying that everything works.

Our VFI structure flips this premise on its head. If the router-developer has a working VFI, and somehow the downstream container manager developer broke something, then their “version” in that VFI is not sanctioned. The burden is now on them to go fix it. However, since the router doesn’t require a dependency update or a rebuild, simply plugging in their fixed version in the VFI, is sufficient enough to get their upgrade pushed into production quite easily.

You’ll also notice how this structure puts our experimentation ability on steroids. Given content-based versioning, and feature-detection, we can plug a thousand different branches, with a thousand different features, experiments, implementations, etc. in a VFI, and move rapidly. If we have to make a breaking change to an API, we don’t really have to either “freeze a branch” or do code lockdowns. We just replace API V1 with V2, and then as various components make their changes, we update those in the VFI and roll out the change reliably, accurately, predictably, and most importantly, easily. We remove the burden on the API changer to somehow coordinate this massive org-wide migration, and yet we also unburden the consumers from doing some kind of lock-step code update.

All the while, we preserve our ability to make strict assertions about every component, and an overall VFI — is it secure? is it building? Is it passing tests? Does it support features? Has something regressed? We further preserve our ability to know what is being used and executed at all times, and where it came from.

Naturally, VFI’s themselves are content-versioned. :-) You’ll find us sending each other sample VFIs like so: `Runtime@b383463cf0f42b9a2095b40fc4cc597443da47f2`

Anyone in the company can use our VFI CLI to expand this into a json-blob, and that blob is guaranteed to be exactly what I wanted someone else to see, with almost no chance of mistake or miscommunication.

Isn’t this cool? We can stay loosey-goosey and experimentally hipster, and yet talk precisely and accurately about everything we consume!

You’ll almost never hear “Does the router work?” because nobody really cares if the router works or not. You’ll always hear conversations like, “What’s the latest VFI that supports scrambling?”, or “What’s the latest stable VFI that supports session isolation?”

Assertions are made over VFIs. We bless a VFI as an overall locked entity, and that is why long-time customers have been getting a monthly email from us with these blobs. :-) When we need to roll out a surgical patch, the overhead is so minimal, it is uncanny. If someone makes a surgical change to one component, they test that component, then publish a new VFI with that component’s version, and test the VFI for overall scenarios. The remaining 8 components that are reliable, stable, tested, see no touch or churn.

### Components are self-describing

#### Runtime Independence

[Alex](https://medium.com/@alexgo) and I are Erlang fanboys and it shows. 100% of Polyverse is built on a few core principles, and everything we call a “component” is really an Actor stylized strictly after the Actor Model.

A component is first and foremost a runtime definition; it is something that one can run completely on it’s own and it contains all dependencies, supporting programs, and anything else it needs to reliably and accurately execute. As you might imagine, we’re crazy about [Docker](https://www.docker.com/).

Components have a few properties:

1. A component must always exist in a single Git repository. If it has source dependencies, they must be folded in (denormalized.)
2. Whenever possible a components runtime manifestation must be a Docker Image, and it must fold in all required runtime dependencies within itself.

This sounds simple enough, but one very important contract every component has, is that a component may not know implicitly about the existence of any other components. This is a critical contract we enforce.

If there is one thing I passionately detest above all else in software engineering, it is implicit coupling. Implicit coupling is “magic”. It is when you build a component that is entirely syntactically decoupled from the rest. If Component A somehow relies on Component B existing, and acting a very specific way, then Component A should have explicitly expressed that coupling. As an operator, it is a nightmare to run these systems! You don’t know what Component A wants, and to keep up public displays of propriety, doesn’t want to tell you. In theory Component A requires nothing else to work! In practice, Component A requires Component B to be connected to it in a very specific magical way.

We go to great lengths to prevent this from happening. When required, components are explicitly coupled, and are self-describing as to what they need. That means all our specs are in the code. If it is not defined in code, it is not a dependency.

#### Build Independence

We then take this runtime definition back to the development pipeline, and ensure that all components can be built with two guaranteed assumptions:

1. That there be a “/bin/bash” that they can rely on being present (so they can add the `#!/bin/bash` line.)
2. That there be a working Docker engine.

All our components must meet the following build contract:

`docker build .`

It really is that simple. This means that combined with the power of content-addresses, VFIs and commit-clouds, we always have a reliable and repeatable build process on every developers’ desktop — Windows, Linux or Mac. We can be on the road, and if we need a component we can do “`docker build .`” We can completely change out the build system, and the interface still remains identical. Whether we’re cross-compiling for ARM, or for an x86 server, we all have a clear definition of “build works” or “build fails”. It really is that simple.

Furthermore, because even our builders are “components” technically, they follow the same rules of content-addressing. That means at any given time you can go back two years into a Git repo, and build that component using an outdated build system that will continue to work identically.

We store all build configuration as part of the components repo, which ensures that when we address “router@<sha>” we are not only talking about the code, but the exact manner that version needed to be built in, or wanted to be built in.

Here too you’ll notice the affinity to two things at the same time:

1. Giving developers complete freedom to do what they want, how they want — whatever QA frameworks, scripts, dependencies, packages, etc. that they need, they get. They don’t have to go talk to the “Jenkins guy” or the “builder team”. If they want something they can use it in their builder images.
2. Giving consumers the capacity to reason about “what” it is someone is talking about in a comprehensive fashion. Not just code, but how it was built, what tests were run, what frameworks were used, what artifacts were generated, how they were generated, etc.

### Where it all comes together

Now that we’ve talked about the individual pieces, I’ll describe the full development/build/release cycle. This should give you an overview of how things work:

1. When code is checked in it gets to be pushed without any checks and balances, unless you’re trying to get onto `master`.
2. You’re allowed to push something to `master` without asking either, with the contract that you will be reverted if anyone finds a red flag (automation or human.)
3. Anyone can build any component off any branch at any time they want. Usually this means that nobody is all that aggressive about getting onto master.
4. An automated process runs the docker build command and publishes images tagged with their Git commit-shas. For now it only runs off master.
5. If the component builds, it is considered “unit tested”. It knows what it needs to do that.
6. A new VFI is generated with the last-known-good-state of working components, and this new component tag is updated in the VFI, and the VFI is submitted for proper QA.
7. Assertions such as feature availability, stability, etc. are made over a VFI. The components really don’t know each other and don’t take version-based dependencies across each other. This makes VFI generation dirt-cheap.
8. When we release, we look for the VFI that has the most assertions tagged around it (reviewed, error-free, warning-free, statically-verified, smoke-test-passed, patch-verified, etc. etc.)
9. In the dev world, everyone is generating hundreds of VFIs per day, just trying out their various things. They don’t need to touch other components for the most part, and there is little dependency-churn.

I hope this post sheds some light on how we do versioning, why we do it this way, and what benefits we gain. I personally happen to think we lose almost none of the “assertions” we need to ship reliable, stable and predictable code, and at the same time simultaneously allowing all developers a lot of freedom to experiment, test, prototype and have fun.
