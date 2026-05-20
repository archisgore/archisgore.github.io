---
layout: post
title: "Kubernetes is cumbersome; not complicated"
description: "Framing and naming the “key problem”; how we got here"
date: 2026-05-20 17:47:51 +0000
---

Ever feel like you’re looking at a thousand resources in a cluster and can’t logically grok it all, where they all came from, what they all do, what purpose they serve etc.? You don’t know who’s reacting to them, what the reactions are, what the ordering/dependencies (if any) are, etc.

So you colloquially say —”*ugh this is so complicated*”.

Instantly you get some experts explaining it all,“*You just don’t think in Modern Concepts. First there’s a Pod, and it’s ephemeral …*”

Basically you repeatedly find yourself maintaining/debugging a large program on a computer you were handed with no context, so you’re asking if there’s source code, config files, the instruction set the processor is running, etc. and by way of answer, you get the definition of a Universal Turning Machine back, “*You’re thinking in cogs and wheels. Let me explain some modern concepts to you. Imagine a State Machine, some infinite Tape and a Head that can either read of write… and everything will become clear.*”

As if (a) you just derived the Turing Thesis from that and figured out it can compute any problem. and/or (b) That if you don’t think in UTMs or Von Neumann architectures, then you’re thinking in 18th century cogs and wheels.

### **Abstractions rely on semantics**

Kubernetes is claimed to be complicated because it is assembly. It may be assembly, but that’s not the reason it is complicated.

Take this pseudo-assembly program and tell me what it does:

```

```

Let me give you a higher-level abstraction:

```
FOR_LOOP(I, )
```
