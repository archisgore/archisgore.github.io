---
layout: post
title: "What is DevSecOps?"
description: "DevSecOps is concerned with pragmatic, implementable, goal-oriented, operational methodologies whose ultimate purpose is not defending ONE…"
date: 2017-08-29 05:39:15 +0000
original_url: https://medium.com/@archisgore/what-is-devsecops-5568304df5b7
---

*DevSecOps will do for Security what DevOps did for Ops. Plan for individual breaches; prevent system-wide breaches.*

There is a lot written on DevSecOps. It’s the next big buzzword that’ll consume our mindshare for the next few years. I have mixed feelings about these branding trends. On the one hand, having seen Agile, NoSQL, REST, and even DevOps become brands during my career, it somehow feels like the original emotion behind those words got lost. On the other hand, having them become living and breathing brands, buzzwords have helped drive adoption and improvements through legitimacy. Even if you believed in [Immutable Infrastructures](https://blog.chef.io/2014/06/23/immutable-infrastructure-practical-or-not/), it helped massively when you could fly back from [Velocity](https://conferences.oreilly.com/velocity) and justify that everyone in the industry was doing it, and you couldn’t be left behind.

[DevSecOps](https://www.meetup.com/Seattle-DevSecOps/) is being defined right now, and I wanted to add the fundamental sentence that defines it above all else. The most clear and accurate definition I have: “***DevSecOps is the goal of maintaining high security of the system, with the acknowledgement that individual secure components and security components can and will fail.***”

That’s my definition, and I’m sticking with it. If you acknowledge that statement to be inherently true, then you can design with that assumption in mind, and make the world a better place.

### How we got to DevOps

Of course I’m going to elaborate a bit further. Let’s take a quick journey back in time and review how and why we reached the DevOps model that we use today. For those of us who were involved in it, it was a journey full of controversy, emotions and a great deal of fear.

At the crux of the controversy was the definitive statement that you take for granted today: “*Despite our best intentions, efforts and abilities, operational systems can and will fail. Networks go down. Machines die. Software crashes. Cables get cut. Power supplies fail. Memory chips have bit-errors. Time is not monotonically increasing in lockstep across all machines.*”

When I was in high school, PC Magazines ran ads for mission critical million dollar server-class hardware. It was very impressive stuff. These machines could run the same identical code in lockstep on two parallel redundant hardware instances, with periodic checksums conducted across them, and ECC RAM, and a whole lot more. You would then have transparent failover to the hot-standby if something went wrong, with no changes to software!

The promise was that if you paid for this million-dollar product, you were absolved from ALL responsibility for any kind of failure. If something did go wrong, you’d just pay an extra million to get triple-redundancy!

I had a mandatory two-semester class on distributed databases and three-phase commits and the CAP theorem. If some transactions did get deadlocked or stuck, all you needed to do was increase the number of phases of the commit, and you pushed the probability of deadlock down further.

At some point, Google came along, and realized that they couldn’t scale with this model. It was not only economically impossible, but physically impossible to expect components could be made 100% reliable — where the probability of failure was an absolute “zero”. Not 1/10¹⁰⁰ (an inverse googol). But zero. Can. Not. Be. Done.

Software that was never intended to handle failure, when it failed, had no infrastructure to diagnose/debug it. Yet planning for failure was taboo. It was the same as admitting you were delivering a known terrible product. Instead of planning on the failure, you should have instead gone back and removed any chance of failure whatsoever, goddamnit!

Google didn’t hit upon something [entirely novel](https://www.infoq.com/presentations/Building-Highly-Available-Systems-in-Erlang?utm_source=infoq&utm_campaign=user_page&utm_medium=link), but they sure helped legitimize it, and I will always be grateful to them for that. They helped legitimize the notion that reliability is a matter of probability, and it is systemic. No component is “reliable” or “unreliable”. Systems are reliable on a scale of zero to one under specific conditions (and that reliability index can change when the conditions under which it operates change.) This applies to literally anything in reality, and admitting something can fail, does not imply fault.

It took a long and hard-fought battle to change to this mindset though, and sometimes through lessons we learned the hard way. Despite our best intentions, we all faced massive outages and paid a heavy cost for them. It is now 12 years since Google published their paper on BigTable, and only in the last few years can you say, “Assuming that the service fails….”, and not imply fault.

That right there, is DevOps! You can spin that in any way you want: Dev doing Ops. Ops doing Dev. Ops as Dev. Dev and Ops collaborating.

In my mind what makes DevOps, DevOps is the acknowledgement that when a system fails, you weren’t the person who didn’t do your job, but rather that it is a natural part of life.

### Which brings us to DevSecOps

If you’re on the offensive side of cybersecurity, you soon go past the redundant, and start your sentences with “Having a persistent beacon on the edge node, we’re going to….”

I’m not making this up. Play a game to test this out. Go find those pentester friends of yours and invite them out for a coffee or drinks. Get talking about their work. If they start their sentence with “If we find an entry…” they buy you a drink. If they start their sentence with, “We begin with a persistent hold…”, you buy them the drink. You’ll become a believer, and you’ll stay sober too.

The security defense world, on the other hand, looks a lot like the Ops world from a decade ago. You’ll notice an uncanny resemblance to the reactions from an ops person from a few years ago. Begin your conversation with, “Assuming a few breached machines, ….”, and brace yourself for a very animated pushback.

You basically just said the following without knowing it: That they haven’t done their job. That they had allowed the keys to leak. That they do not understand how to secure systems. That they used sub-standard processes. That they screwed up.

There are a few problems with this narrative though.

1. **The Data doesn’t support it**: Just by following tech news, we all know that despite our best intentions, standards, compliance, methods, processes, and paranoia the number of real-world compromises are only increasing. I consider this almost intuitive and won’t further belabor this point.
2. **Internal Defenses are difficult to justify (if not outright impossible):** The clear and immediate danger here is that post-exploitation defenses become impossible to justify. Any kind of proposal for internal defenses is an accusation that external defenses are insufficient or ineffective. This creates massive hostility. This creates a very dangerous self-fulfilling prophesy. If someone does break in through the perimeter, they could potentially “get everything”. That means the idea of even acknowledging that such a possibility exists, becomes unthinkable! So you can’t bring it up. If you can’t bring it up, you can’t address it. The cycle feeds itself — until someone runs away with ALL your data.

Traditionally Security/InfoSec has been seen as a burden by most Ops/Dev teams because security is a culture of saying “No”, as opposed to “Instead of”. When things go wrong, it is typically the dev/ops people who get blamed for not fully implementing security recommendations, guidelines or policies, however security teams rarely prioritize those priorities on a roadmap or timeline. Everything is always important at all times. And yet due to constraints of physics, something has to come first, and something has to come next. I know of nearly no company where, if you had to start a new project, you could get a signoff for a six month delay in your deliverable, so that you could first invest in a secure foundation. In general, I want to see that the feature is going to be worthwhile, before I care about securing it. That’s just the way life works. The challenge is, even if we DID get that six-month sanction, security teams will rarely be able to put their recommendations on a timeline — putting one thing BEFORE another thing.

For a DevOps person, this means that security only ever adds more workload, but very rarely takes responsibility. You physically cannot do everything all at once. SOMETHING has to come first, and the minute you pick one thing as being before another, you get criticized for not giving that other thing the importance it deserves. There is literally no way to not get criticized when dealing with security recommendations.

So I’m taking a stand and expressing an opinion. This opinion should be subject to debate and discussion, but we’ll never have those unless I say some uncomfortable things we all have to face. This is my

### **DevSecOps manifesto**!

#### Everyting is hackable

This is probably the most difficult emotional hurdle we’re all going to have to cross. All of us are vulnerable to phishing attacks and social engineering attacks. Our machines are vulnerable to a whole host of attacks. This is merely a fact of life. It happens.

We can have done all that we could. We can have done everything right. Despite all that, we will remain hackable. There could have been an oversight on someone’s part, or it could have been bugs in the code, and it could have been an internal malicious actor, and more than often, it could have been an internal non-malicious actor as well.

The bottom line is — how or why comes later. We have to get comfortable with the notion that systems will get compromised and people will get compromised despite our best efforts.

#### Know what we don’t know

This is perhaps the second-most difficult emotional hurdle we all have to get comfortable with. I’ve seen a lot of threat models in my life, and every single one of them was [made up of nails](https://en.wiktionary.org/wiki/if_all_you_have_is_a_hammer,_everything_looks_like_a_nail). I have never seen a threat model containing an answer that said, “darned if I know!”

Typically, threat models are rhetorical. We already have an answer in mind, so we ask the leading question, “How do you ensure integrity of the payload?”, and we write, “By using PKI signing!”

Attacks like WannaCry and Heartbleed have shown us that we fail to ask questions to which we don’t know the answers. For example, “How do you defend against a logical bug in a dependant library?”

We don’t have an answer. We have a lot of fumbling responses like fuzz testing and code reviews. But we don’t have a slam-dunk answer, so that question rarely makes it into the threat model. We instead frame it as, “How do you ensure your systems are patched?” to which we have a satisfactory, “by performing frequent patching!”

The truth is, avoiding the question doesn’t make it irrelevent. The offensive attacker knows the threat model, and just because we didn’t write, “Darned if I know how to fix it”, doesn’t mean that they are ignorant of it. Is it better to acknowledge that we simply don’t know a lot of the answers and at least speak the same language as an attacker, or to not mention it because we don’t have an answer?

#### Learn to prioritize

Given that we have a thousand security tasks to accomplish, and knowing that this will never be a “solved problem”, we need to get comfortable doing one thing first, and another thing next, and then another thing after that. Whether it be passwords, MFA, PKI, whatever your poison. They can all be equally important. They can all be equally necessary. However, in physical spacetime, one of those things have to get prioritized and DONE.

#### Focus on the goal: We are defending the system, not a component, a machine or a person.

This is the goal we’re all fighting for. Instead of viewing the problem as an all-or-nothing high-stakes scenario, where a single component breach results in a complete system-wide breach, we need to do what DevOps did to old-school Ops.

Components can and will be broken. That’s okay. Assuming it HAS happened, how can we design systems so that entire systems aren’t compromised and taken over? I don’t know the answer to this, and that is acknowledging my own point — I know what I don’t know. But I know what I want.

***DevSecOps is concerned with pragmatic, implementable, goal-oriented, operational methodologies whose ultimate purpose is not defending ONE host or ONE person from attack, but to prevent the SYSTEM from being compromised.***
