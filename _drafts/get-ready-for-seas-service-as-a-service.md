---
layout: post
title: "Get ready for SeaS: Service as a Service"
description: "How Kubernetes could make it arrive a lot sooner"
date: 2026-05-20 17:47:50 +0000
---

I forgot where I read this, but a particular article I read about iPhone’s success in the early 2010’s gave me a new perspective on what the iPhone really did, and why remains such a runaway hit.

It wasn’t features, or the touchscreen, or even the capabilities. But we all know that already. For the first time ever, at a fixed (but expensive) cap of $700, you could get the exact same phone that every CEO, executive, investor, yacht owner and private-jet-owner would want!

The iPhone was **complete, without being a least common denominator**.

There are two approaches when you want to solve for Everyone.

The easy approach to find the least common denominator across every story, and leaving completeness as an “implementation detail”. Phone calls and text messaging are a given, maaaaybe the ability to play a game like snake. Then there are spinoff models with addons like GPUs or cellular data (yeah, that was a separate thing).

![](/assets/images/get-ready-for-seas-service-as-a-service/image-2f32ae0ad038.bin)

The significantly more difficult approach, and one that usually changes the game, is one where you move the least common denominator itself where it completes multiple user stories. The iPhone went further and provided Marquee apps like Maps that immediately solved problem, and set the tone for others to follow. The design documentation felt like help to make your own beautiful gorgeous Maps application, instead of an enforced draconian crutch.

#### Startups need the same number of services, they just need less instances of each type

What the casual observer, and most developers sitting in BigCorp gets wrong about SMBs is that if you’re a company you need the complete set of Vertical services to function. They just need different horizontal scales of each.

I use startups as an example because I work at one and know this first hand. t is incredibly humiliating to hear the phrase, “*At your scale you don’t need it.*”

Yes we need it. We can’t afford it, especially not for a fixed known price tag that we can plan for and account for. We need the same stuff as BigCorp does: accounting, HR, source management, release management, documentation, API versioning, rolling upgrades, etc. etc. We need it all.

What we need from each type, are fewer instances. We might have fewer people in our identity system. We might have fewer repositories for code. We might have fewer build jobs.

When BigCorp developers build stuff “SmallCorp”, they frequently miss the context in which they need something. They have an org of hundreds of people managing identity. Another org managing backups and disaster recovery of the core data. Another org that buys the physical hardware (or rents it from a cloud) and plugs it in.

Today’s services are very much like the feature phone market of 2006. We either get the least common denominator
