---
layout: post
title: "Polymorphic Linux in ten seconds"
description: "Replace:
FROM alpine
with
FROM polyverse/polymorphic-alpine-base"
date: 2017-09-23 18:46:12 +0000
original_url: https://medium.com/@archisgore/polymorphic-linux-in-ten-seconds-74ed8a8d8333
---

Replace:  
`FROM alpine`with  
`FROM polyverse/polymorphic-alpine-base`

*Pro-tip: You can substitute* `alpine` *with* `centos` *or* `ubuntu`*.*

Now let’s get into the details of what this is, how it works, and what the future is.

### CentOS, Alpine and Ubuntu images

There are three images which provide replacement base replacements for stock CentOS, Alpine and Ubuntu:

Alpine: <https://hub.docker.com/r/polyverse/polymorphic-alpine-base/>

CentOS: <https://hub.docker.com/r/polyverse/polymorphic-centos-base/>

Ubuntu: <https://hub.docker.com/r/polyverse/polymorphic-ubuntu-base/>

Each of these provides you with a Docker Base image, that is pre-scrambled, and also re-scrambles itself when you build your derivative images.

If you add any new packages, using the package manager (yum, apt, apk), those new packages will also preferentially come from scrambled repositories whenever available.

This, is how easy it is, to get your own unique, scrambled version of Polymorphic Linux, and start using it!

If you want to dig further down, each of these has a corresponding public Git Repo, that explains how it works, and how trivial it is to subscribe to Polyverse repositories from ANY Linux host.

### Background

For over two years, we at [Polyverse](https://polyverse.com) have been building out our scrambling compilers. [1] There are usually three aspects to any technology. The science behind must work, the product has to be fully closed in a functional sense, and the user experience must be intuitive. The final piece is always the most difficult to nail down, especially when introducing something new to a market.

Cognitive load is reduced, not necessarily by making something simple, but by ensuring that it is familiar and more importantly, honors the de-facto contract; when it must break the contract, it must do so explicitly and conspicuously. [2]

This led us to the simplest answer out there, which was to provide packages to your package manager, and the benefits speak for themselves:

1. We don’t install/inject any daemons or kernel modules. All questions around userland/kernel space just go away. All concerns around live-modification of running processes no longer exist.
2. Running package updates is a well-understood problem. It has been understood for at least two decades now.
3. Your package manager becomes your friend. Verifying source, verifying integrity, performing safe rolling upgrades (preserving config across updates, not replacing live binaries in use, etc.), and so on just happens as it has always happened.
4. Improvements are seamless and transparent; so is fall-back. When scramble new packages they get picked up with no additional effort or changes. If we don’t scramble a particular package or version, it doesn’t stop the system from working.

### How we did it!

#### Drop-in Replacement Package Manager repositories

The way we provide and ship Polymorphic Linux, is by providing drop-in replacements for the default package repositories that power CentOS, Alpine and Ubuntu. We literally recompile, multiple times, every package in those repositories, and provide the same package, but with scrambled binaries, as stock CentOS, Alpine or Ubuntu would have shipped.

When you open the github repos for all three images, you’ll notice the simple difference between the base images and these three images, is a script called `rescramble.sh`.

You may open up that shell script for either repository to see how to subscribe to that particular distribution’s scrambled repository from us.

It truly is THAT SIMPLE! No daemons. No agents. No kernel modules. No live-scrambling of pages in memory. No clever dynamic analysis.

### *Footnotes:*

[1] Let’s talk about what Scrambling Compilers do, and why they matter, what attacks they prevent, etc. Especially the big question, how is this different from [ASLR](https://en.wikipedia.org/wiki/Address_space_layout_randomization)?

This is a very brief overview. Do get in touch with us to learn more, and we’re in the process of writing detailed white papers for public consumption.

At the very basic level, ASLR offsets binaries in overall memory space, but within a single binary, all offsets remain constant. For instance, if “strcpy” was 10 bytes ahead of “memcpy”, then that offset remains constant throughout the lifetime of that binary, with ASLR on or not.

[KARL](https://www.bleepingcomputer.com/news/security/openbsd-will-get-unique-kernels-on-each-reboot-do-you-hear-that-linux-windows/) is a recent feature in OpenBSD, that only applies to the kernel so far. It goes a step further than ASLR by randomizing internal object placement within the kernel (so knowing the base offset isn’t enough.)

Polyverse scrambling does a lot more, and aims to apply it to ALL packages on Linux.

[2] I have always detested it when people make a statement, and then add a subtext listing all the exceptions. It sounds like those pharmaceutical ads on TV that then mention quickly how the drugs might cause illnesses.

One of the strict rules at Polyverse is to mean what you say and say what you mean. If we say HTTP, then we support HTTP and will interact and play nicely with any routers, firewalls, proxies, web servers, browsers, clients, etc. that understand HTTP. No exceptions. If we must make a change to HTTP to support something weird, that is entirely ALLOWED and encouraged, under the strict condition that it must not be called HTTP. You’ll notice underlying propensity for [Content-based Versioning](http://conver.io) here as opposed to label-based versioning.

This means that we don’t waste valuable time on the obvious, instead prioritizing time for discussing that which is not. It reduces fear, uncertainty and doubt. When someone says “JSON”, we just move on. It is implied that you can go to [json.org](http://www.json.org) and you’ll know all you need to know about what they meant.
