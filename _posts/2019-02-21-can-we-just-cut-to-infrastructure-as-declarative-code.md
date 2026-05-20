---
layout: post
title: "Can we just cut to Infrastructure-As-Declarative-Code?"
description: "The missing link between Declarative Infra and Infra-as-Code"
date: 2019-02-21 19:43:53 +0000
original_url: https://medium.com/@archisgore/can-we-just-cut-to-infrastructure-as-declarative-code-3b0b44fa02
---

Everyone who is allured by the promise of Declarative Infrastructure, “*declare what you want, and don’t worry how it happens*”, eventually seems to end up at [half-baked](https://blogs.vmware.com/cloudnative/2019/02/05/welcoming-heptio-open-source-projects-to-vmware/) [verbose](http://leebriggs.co.uk/blog/2019/02/07/why-are-we-templating-yaml.html) [clumsy](https://gravitational.com/blog/kubernetes-kustomize-kep-kerfuffle/) templating.

Nobody looks at 50 yaml files (with templates even) and can read a crisp declaration: *this is a secure, throttled, HA wordpress instance! It’s so clear and obvious!*”

A long-overdue reaction to verbose templating is the new allure of — Infrastructure-As-Code. [Pulumi](https://www.pulumi.com) and the [AWS CDK](https://github.com/aws/aws-cdk) offer these beautiful compact clean parameterized abstractions (functions, objects, etc.)

There is discomfort though from the Declarative camp — was the promise all wrong? Was the implementation wrong? What gives? We know we should want it… but the imperative definitions really are readable.

I’m here to tell you the world is offering us a false choice — always has. What if I told we could have “Declarative Systems” and “Infrastructure-As-Code” all in one? Let me explain…

#### We’re confusing “Static Configuration” with “Declarative Programming”

I’m going to go after (allegedly) the “worlds first system that allows you to declaratively provision infrastructure”.

Kubernetes YAML is not and never was Declarative Programming! It was a magic trick you bought into because of all the grandiosity that came with the sales pitch.

***Kubernetes is a Statically Configured system***. What were DOSs `.ini` files in the 80s, `/etc/*.conf` files in the 90s, are the YAML for Kubernetes. When we `kubectl apply` we are writing bunch of strings to a KV store with pomp and grandiosity that made us believe we were doing “Declarative” something something. Don’t hate me just yet — because if we accept this, we can build a world that is oh so much more beautiful!

#### Infrastructure doesn’t have an Assembly Language

> Writing a “Higher Level Language” on top of Kubernetes’ alleged “Assembly Language” makes about as much sense as writing C using regular expressions.

Even if Kubernetes were the kernel, YAML is NOT the Assembly Language, because it is missing ***The Language***. Most charitably, the Kubernetes resource model would be “[Registers](https://www.tutorialspoint.com/assembly_programming/assembly_registers.htm)” for a language that doesn’t exist; and they’re really just data constants not even registers.

You know you can write a regular expression library in your favorite programming language — Javascript, C#, Lua, Elm, Ballerina, whatever. Can you write your favorite programming language in Regular Expressions?

Now compare Assembly Language to Java, Javascript, C#, C++, Elm, Go, Rust, etc. You can write any of these in any of the other ones.

That’s the difference — Assembly Language is not a “Lesser Language”, it is a “Lower Level Language”. It can do everything the others can do — no more, no less.

Writing a “Higher Level Language” on top of Kubernete’s alleged “Assembly Language” makes about as much sense as writing Java using regular expressions.

This is the essence of why templating looks awkward, and Infra-As-Code looks better, but… feels like sacrificing the very promise that attracted you to Declarative systems in the first place.

#### What you want is a Declarative System + The Language => Declaractive Programming!

[Declarative Programming](https://en.wikipedia.org/wiki/Declarative_programming) is neither new, nor do you need to have planet-scale problems for it to be useful. If you were tempted by Declarative Infra for the promise of describing your complete apps in a repeatable, portable, and all-in-one-place style, what you wanted was: **purity/idempotence**, **parameters**, and **closures**.

You were promised a **Declarative Assembly Language**, but you were given [Data Registers](https://www.tutorialspoint.com/assembly_programming/assembly_registers.htm).

Imperative programming gives you better abstractions than templating, but it still doesn’t understand them — you are still expressing HOW you want Data generated, not WHAT you want as an overall Goal.

There is a better way! There is a **Declarative Way** of writing programs, where **Predicates** are what would be *loops and conditionals* in imperative programs. **Relations** are what would be *functions* in imperative programs. **Facts** are what would be *data* in imperative programs. **Assertions** are what would be t*ests* in imperative programs. If you haven’t already, Play with [Prolog](https://swish.swi-prolog.org).

Declarations are not these dead blobs of static YAML! They’re living breathing encapsulations of understanding and knowledge that makes the platform grow and be better!

### A Declaratively Programmed Infrastructure Platform

I wrote a [mini-spec on twitter](https://twitter.com/adrianmouat/status/1097967361691394055) and I want to get past the theory and describe what a proper Declarative App/Infra Platform would look like (whether in/on/above/under Kubernetes or not.)

#### **Desires**

Desires are what you express. The system doesn’t change them, touch them, modify them, mutate them, etc. Strictly no admission webook mutation api aggregation server controller whatsoever on a desire. You can express anything in the world you want. You can Desire to be paid 1 million dollars. You can Desire a Ferrari.

In today’s world, what you `kubectl apply` would always be a desire. It represents nothing more nothing less, and nobody gets to change it, modify it or argue that you shouldn’t want what you want.

#### **Facts**

Facts are things the system simply knows to be true. A fact would be what is the `/status` sub-resource today or a Node. No more weird ugly resource/sub-resource bullshit that everyone is modifying and awkwardly versioning with ever-complex merge algorithms. Just straight up “Fact”. I “Desire a Pod” so the system gave me the “Fact of a Pod”. Two independent first-class entities.

#### **Predicates**

Predicates are “knowledge” the system has as it learns. A Predicate applies constraints, adds information, removes information, but in a DECLARATIVE way.

For example, today if you knew all Scottish Sheep are Blue, you can’t declare that knowledge to any Declarative Infrastructure. You have to enter EACH sheep in Scotland as being blue either through templating or through a “real language”. Not only is one verbose, one clumsy and one non-declarative, the real travesty is that valuable *knowledge was lost that others cannot benefit from*. Nobody else can read the code and infer that you really wanted to state that “All scottish sheep are blue.”

In Declarative Programming, though, you can have it both ways! Enter Predicates. You can at once know whether any particular sheep is blue, and also remember the general rule that all Scottish Sheep are Blue so others can benefit from it. You don’t give one up for the other.

More concretely let me write you some predicates that the system would understand first-class. These aren’t some clumsy custom controllers responding to a Shared Informer using a ClientSet with CodeGen’d Golang API packages and iteratively using level-triggered reactions to set annotations. Ugh so many words! No no, these are fundamental Declarations to the Assembly **Language** of the system! This is the system’s **Language** not static configuration.

- **All Pods not fronted by a service -> Ready to Garbage collect**

*(Note that I didn’t write all those Pods should be marked as Ready to Garbage Collect. They ARE ready to garbage collect — you don’t tell the system WHAT to do, simply what you KNOW.)*

- **All Services with HTTP endpoint -> Invalid**
- **All Pods with older-than-30 days SSL keys -> Invalid**

Once declared, the predicates teach and improve the system itself. The system understands them. Not in operators or controllers or some third-party templating place.

#### Relations

Finally, what makes all this work magic is Relations. Relations are what teach the system how to accomplish Desires. A Relation teaches it how to take a Desire and relate it to more Desires and/or Facts.

The system simply breaks down high-level Desires, until all Desires have a Relation to a Fact. Then it manifests those Facts. It can also tell you when a Desire cannot be met and why. Is a Predicate getting in the way? Is a resource depleted?

Let me illustrate:

- **Relation:** Desire: Exposed Service(ip) -> Fact: IP Open to Internet
- **Relation:** Desire: Secure Service(domain) -> Desire: Secure Proxy(domain, ip\_of\_insecure\_service, domain\_cert) + Desire: Insecure Service + Desire: Domain Cert(domain)
- **Relation:** Desire: Secure Proxy(domain, ip, cert) -> Fact: Nginx Proxy(domain,ip,cert)
- **Relation:** Desire: Insecure Service -> Fact: Insecure Service
- **Relation:** Desire: Domain Cert(domain) -> Fact: Domain Cert(domain)

Now I collapsed a few steps here, but it doesn’t matter. You get the point on how to Relate an **Exposed Secure Service**, to an **Insecure Service and Cert Generator**.

This is all we need. Let’s bring it all together!

#### A System with 1000+ Microservices at Planet Scale described As Code 100% Declaratively

Let’s look at how the cluster comes together:

- Vendors/Sysadmins/Whomever provide Relations that convert Desires to Facts. This is below the app-interface you are supposed to see. If a Desire doesn’t become Fact, the Vendor/Admin/Operator is paged. A clear boundary.
- InfoSec/Business/Compliance injects predicates. They define what constraints they want. What can and cannot be true.
- App-Developers provide a Desire. That is all they do. They get back if it can or cannot be met. They get back if it cannot be met, WHY it cannot be met — a predicate got in the way, missing Relation i.e. don’t know how to break down a certain Desire to Facts, A Desire->Fact relation errored and here’s the error details.

Now we have a Declaratively Programmed Infrastructure — where knowledge is not lost, we get full programming, we get full declarative-ness, and we get even more things:

1. We can ask WHY something exists. Who desired it? Are they allowed to desire it?
2. We assert things and make overarching statements. We can say “All Pods in EU always store data in EU”. We can simply MAKE such a powerful statement.
3. We can add constraints and not scan/validate/audit. If a constraint exists, it simply makes certain Facts untrue, i.e. they are unmanifestable.
4. We can compose higher Desires over lower Desires.

If I were sold THIS Declarative system with the lowest Assembly Language today, I would buy it.

Whether I’ll ever get to use one, I don’t know — but I sure hope so.
