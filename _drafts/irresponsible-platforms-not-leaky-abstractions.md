---
layout: post
title: "Irresponsible Platforms, not Leaky Abstractions"
description: "Or “This should have been a library.”"
date: 2026-05-20 17:47:51 +0000
---

My roughly periodic rant: Libraries > Frameworks > Platform

Are you constantly defending your platforms and frameworks with: “All Abstractions are Leaky” or “It Depends” or “I can’t really speak for the underlying stack”, etc? You probably built a library and marketed it as a platform. You probably need to learn to say NO! a lot louder and feel okay with not everyone loving you and needing you and wanting you.

#### Libraries

Libraries are whatever behavior you choose to package that you think is reusable and others may benefit from not re-doing. They can use it, integrate it or not based on their choice. You have no responsibility whatsoever at best, and at worst you take responsibility for the code you wrote and you never have to make disclaimers. You never have to say “It depends”. Packet filtering, parsing, sorting, task scheduling, compiling, running — everything can be a library. Just a porridge of code.

It’s like selling milk, sugar, eggs, yeast, flour. You describe exactly what they are, and their quality, expiration, etc. But nobody will blame you if their pizza sucked.

**Rule of Thumb:** Is this code that can be used for anything by anyone for everything everywhere? Is this really really hard code you wrote? Did this take cleverness? Are you proud of it? Do you think this can help everyone? **Congrats! It’s a library!**

#### Frameworks

Frameworks are opinionated structures that others run. You are not responsible for the environment, but you ARE responsible for advising people what environment it should be used in — and then owning up when the outcomes don’t match promises. A good framework owner has loud alarm bells when the environment doesn’t match the platonic ideal. 99% of bad customer experiences happen when frameworks feel shy about saying what they DON’T do, or hope they can be extended.

Networking stacks are an example of good frameworks. Whether an ethernet collision or radio interference happens, you don’t get back a some abstract “IUnderlyingThingyError” that depends on what driver you used with instructions to talk to driver-owner and figure it out. If they need to enforce some structure between the TCP layer and Physical layer, they’ll do that, or they’ll loudly tell you THIS NETWORKING PIGEON IS VERY COOL BUT NOT SUPPORTED! GO USE A LIBRARY YOU BIRD-LAWYER!

Why they’re frameworks and not platforms, is there’s no purpose to networking by itself, but they ship you more than just some code they wrote. Operating Systems are similarly good examples of frameworks.

This is like buying a packaged cake-mix. You won’t get bread for lots of reasons, but the packaging says that. It never says “Oh it depends… you could add some gluten, but then you need a high-strength yest…”

How many tech stacks do we get these disclaimers for? For sure someone can adopt a [DNS service as a database](https://betterprogramming.pub/apparently-you-can-use-route53-as-a-blazingly-fast-database-dd416b56b005) and nobody will stop you, the same as a competent baker can turn a cake pre-mix into a load of bread. However if you ask a DNS framework provider, they’ll never say “It depends” — they’ll ask you to either consider something else, or never ask them again.

**Rule of Thumb:** Can you upfront state at least one problem statement in one environmental condition, wherein using your code will solve that problem? Without any cognitive load, no decision-making, no caveats, no configuring? When that expectation is unmet, can a user call you and hokd you responsible and accountable? **Congrats! You have a Framework!**

Yes, I know, it’s painful. I wouldn’t want to do it either when I can avoid it. That’s why you should build a Library.

NOTE: You can still state for every other purpose than that, your code is still as good as a library.

#### Platforms

Platforms are simply **committed outcomes for a promised cost**.

If you have to tell me some outcome depends on how I choose to configure it or what SQL server I use, you’re selling me a Library.

Platforms give you, the owner, an incredible amount of power. You pick what dependencies you want to take. You decide whether you use Microservices, Service Meshes, Blockchain or Quantum. (Obviously you MUST use AI - even you have no choice in that. If I’m not using ChatGPT to interface with my SQL server, I’m not getting enough SQL server.)

Aside from cost, a Platform doesn’t put any cognitive load on the user. The tradeoffs? You need to make them. The good news? You absolutely get to ask questions of a customer. You get to understand their use-case. You get to understand their desired outcomes. They want you to know, because they know you’re taking responsibility.

**Rule of Thumb**: Are you making a commitment of delivering some outcome for a given cost? Aka “I’ll serve 100mil web requests/sec. for $10mil”, or “I’ll store 100gb of relational data for $10”? **Congrats! You have a platform.**

#### Build a Library! Please!

It is almost always a great idea of build a Library first. They comes with the least liability, give you the broadest reach, and the most escape hatches. They’re just really really awesome. You never have to hit twitter talking about how all abstractions are leaky, and people are holding your abstraction wrong.

It is far easier to promote frameworks that are patterned uses of said library. People understand the context better and understand the escape hatches.

Platforms are significantly easier than Frameworks any day of the week, but need a lot of work. You own EVERYTHING. Take that as empowerment or a threat.
