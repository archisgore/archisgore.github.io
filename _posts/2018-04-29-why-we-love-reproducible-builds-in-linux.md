---
layout: post
title: "Why we love reproducible builds in Linux"
description: "Debian is moving to “Deterministic/Reproducible Builds”, and we love it!"
date: 2018-04-29 17:36:01 +0000
original_url: https://medium.com/@archisgore/why-we-love-reproducible-builds-in-linux-83e77b01e139
---

I’m up in Bellingham, WA for [LinuxFest Northwest](https://linuxfestnorthwest.org). A very frequent question we’ve received at the [Polyverse](https://polyverse.com) booth is what we think of Reproducible Builds in Debian, and whether we contradict the idea.

The immediate answer is not only that we don’t contradict this, but if you ever read my post on how we build everything at Polyverse, you’ll find that I’m a [passionate advocate of determinism, reproducibility, content-addressing and more](https://blog.polyverse.io/how-we-build-software-at-polyverse-49bbd86d817). I wouldn’t have it any other way.

That doesn’t make sense though, does it? Well… it should by the time you’ve read this.

### Our Polymorphism is [random](https://en.wikipedia.org/wiki/Randomness#Misconceptions_and_logical_fallacies), not [unstable](https://en.wikipedia.org/wiki/Stability_theory)

We tend to use the word random a little… randomly. Randomness is unpredictability of an outcome, not misunderstanding of the outcome. Randomness derives from having good control, understanding and predictability of a process.

That thing where you don’t understand what’s happening, why it’s happening and what to do about it… that’s called “instability.”

Each build of Polymorphic Linux is stable. Given the exact same entropy source, with the same input events, it generates a stable, predictable, reproducible binary. It doesn’t “screw up” things without understanding or control. It scrambles things based on an unpredictable source of entropy, but with a reproducible source, so is stable and reproducible.

### A question of identity

The real challenge going forward, for all of us, is that of the “identity” of code, programs and systems. Today we benefit from the mob mentality of risk mitigation. We don’t for sure know that our binaries have or don’t have a particular vulnerability. Vulnerability scanners have no idea what source code a particular program came from.

We utilize a bit of “trust me” and a lot of “but they’re doing it too!” Meaning, so long as everyone else is taking a certain fact at face value, hopefully they’ll be blamed before me, or atthe very least nobody will tell me I should have done anything different.

Semantic versioning is [famously terrible for identity assertions](https://github.com/semver/semver/issues/148). They are arbitrary strings placed in metadata.

The contemporary method is to verify identity by verifying hashes of binaries. This only works if we have reproducible builds, otherwise the same person providing me with the binary is supplying the hash — and also telling me what vulnerabilities it has. Which may make me feel good, but provides no practical security benefit. This is what Debian is addressing with this initiative. We love it, we endorse it, and we’re all for it!

In due time, you’ll be able ask your scanner tool whether a binary came from specific source code, not whether it simply had some hash.

### Instability is dangerous and we should all fight it

Instability is dangerous. Instability is the classic situation where you might be told it’s a feature, not a bug. Trust me, it’s a bug. We can only have good Moving Target Defense when we have stability in the tools upon which we build it.

What Debian is doing is necessary, important, valuable, and we love it!
