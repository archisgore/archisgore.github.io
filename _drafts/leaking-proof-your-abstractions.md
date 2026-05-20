---
layout: post
title: "Leaking-proof your abstractions"
description: "using strong explicit contracts"
date: 2026-05-20 17:47:51 +0000
---

If you’re an engineer, especially someone who’s chronically worked at a large company, there are two phrases you say a lot: “It depends” and “All abstractions are leaky”.

There’s nothing really wrong with that. Everything in life depends. Whether you pick summer tires, winter tires or all-season tires — there are tradeoffs. What window sizes you pick for your TCP buffer. What size and timeouts you pick for your caches. So and and so forth. The list of contextual tradeoffs is endless.

What most commonly gets masqueraded behind these two phrases though are very avoidable frustrations. What is elixir for a few years, becomes poison in latter years when you’re stuck with legacy, unable to make simple changes due to cumbersome lockins.

To avoid everyone coming at me with a “Well Ashktually…” let’s just say all of your Leaky Abstractions are the Good ones. The Best ones. So let me talk about the Bad Leaky Abstractions that other, lesser people, write.

#### What are “Bad Leaky Abstractions”?

> A Bad Leaky Abstraction is: any behavior of a system that is based on knowledge of the inner workings of any of its dependencies or dependants.

Consider an HTTP library that
