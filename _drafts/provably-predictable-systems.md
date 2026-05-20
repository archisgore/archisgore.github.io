---
layout: post
title: "Provably Predictable Systems"
description: "also: what “Leaky” abstractions really are and why they suck"
date: 2026-05-20 17:47:51 +0000
---

I was asked a while ago, “*You can’t design provably correct systems, can you? How can you fault <insert company I had made a snide comment against> for missing the edge case?*”

I think a huge nuance when building systems is that while you can’t build a provably *Correct* system (i.e. one that only results in a Desirable Outcome), you can build a provably *Predictable* system (i.e. one that either results in a Desirable Outcome, or fails in one of a countably finite set of States.)

When systems are Unpredictable, it usually comes down to Leaky Abstractions (more on those later .)

“Correct” vs “Incorrect” is a matter of definition. It may have as many or as few attributes as you care to define. For example, a “Realtime system” isn’t a really really really fast system — it’s a predictable-in-time system. The way a program strives to be predictable in memory usage, or order-of-magnitude of computations (Big-Oh), it may have an additional requirement of being predictable in wall-clock time.
