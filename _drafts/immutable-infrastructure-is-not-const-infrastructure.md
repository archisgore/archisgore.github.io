---
layout: post
title: "Immutable Infrastructure is not“const” Infrastructure!"
description: "Immutable Infrastructure is a confused teenager just learning the difference between constants, literals and immutability."
date: 2026-05-20 17:47:51 +0000
---

You may also read my rant on how we don’t really have [Declarative Infrastructure](https://medium.com/@archisgore/can-we-just-cut-to-infrastructure-as-declarative-code-3b0b44fa02) either.

#### A quick recap of Constants vs Immutability

Let’s understand how immutable data is different from constant data and literal data.

In most program languages is a way to define a “constant”. Something we know a-priori and we want to refer to symbolically later on. The key property of a constant, is that it is known at compile-time at the very latest (it may never change post-compile.)

An immutable, on the other hand, is a runtime property that something cannot change in-place.

If you’re a Java programmer, consider your `String` class. Can you generate new strings when a program is running, or are you restricted to strings you only knew at compile-time? A program that allowed the latter, would be pretty useless, wouldn’t it?

And yet, a `String` is immutable. What this means is that once created, you may not change *that particular String;* you may generate as many new strings with modified values as you want — and they may even depend on the value of that string. Other strings may depend on the values of those generated strings as well.
