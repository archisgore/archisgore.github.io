---
layout: post
title: "How Polymorphic Linux stops attacks when everything else has failed"
description: "Using offensive methods against the attacker"
date: 2017-11-30 03:05:16 +0000
original_url: https://medium.com/@archisgore/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed-cd8d6b2d669f
---

![](/assets/images/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed/1_oeIo5XVQt9lHyW1UPM6yyw.jpeg)

It’s time to address the elephant in the room. You can read my binary analysis series ([Part 1](https://blog.polyverse.io/lets-craft-some-real-attacks-8efac7b3df90), [Part 2](https://blog.polyverse.io/fun-with-binaries-4361128556a4), and [Part 3](https://blog.polyverse.io/why-aslr-stops-rop-attacks-8e961ca6951e)) for a detailed recap. We’re going to use the [open source](https://github.com/polyverse/binary-entropy-visualizer) [entropy visualizer tool](https://analyze.polyverse.com).

For a quick recap, [ASLR works](https://blog.polyverse.io/why-aslr-stops-rop-attacks-8e961ca6951e) something like this:

![](/assets/images/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed/1_XWz5kE1e6yCuVdqqIXqbMQ.png)

All instructions/gadgets are offset by a fixed amount

This is the same as trying to dodge bullets. You move faster than the bullets come at you.

![](/assets/images/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed/1_p5mbYuaYeak8iL0uvJkmqA.gif)

It works… until it doesn’t!

You have the same binary that everyone else has. You just move it around in different places in memory with a single degree of freedom. It works better than staying absolutely still. However, with one offset discovered (a single 32-bit or 64-bit integer), ASLR is effectively broken. All you do, as we saw in the [previous post](https://blog.polyverse.io/why-aslr-stops-rop-attacks-8e961ca6951e), is add that offset to your ROP chain and ASLR is all but over.

This is Polymorphic Linux:

![](/assets/images/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed/1_G3x-bIVgJ9vEuScAX9MtPA.png)

Sample in “CentOS 7.4” directory of the git repo.

More graphically, this is what is going on:

![](/assets/images/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed/1_8Iw4QV18R9dtbpyG-0Ejqw.gif)

Don’t try to dodge the ROP chains. That’s impossible. Instead, only try to realize the truth — there ARE no ROP chains.

If you go to the [CentOS 7.4 samples](https://github.com/polyverse/binary-entropy-visualizer/tree/master/samples/centos7.4) directory, you’ll find two binaries. A standard libc-2.17.so that ships across all of CentOS, and a libc-2.17-scrambled.so, a single binary that ships as part of the Polymorphic Linux repositories per day (this particular binary cannot be downloaded again — the [free tier repos](https://polyverse.com/polymorphic-linux-installation-guide/) get a new scramble each day.)

The two binaries are compiled from the exact same source code. They perform semantically identical functions. Since they are compile-time scrambled, they have no magical code-rewriting at runtime, thus no performance impact. However, you’ll notice that the binary on the right has different numbers of ROP gadgets, different types of ROP gadgets and offsets that follow no pattern.

Out of 82,000 or so gadgets, roughly 900 survived in the same locations. Those that got moved, moved very abruptly and not by a fixed offset.

![](/assets/images/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed/1_D65ha_PrdtTnd174gmRkrg.png)

Same function. Different binary. 904 gadgets out of 82,000 survived.

This is **Polymorphic Linux**: an ever-increasing set of Linux packages with polymorphic binaries!

But hold on… we haven’t gotten to why it works yet. Let’s begin with a good baseline — why does ASLR even work; and also why it is defeated so easily.

#### ASLR is a one-dimensional change-of-origin transformation

The offset transformation used by ASLR, mathematically expressed, is a “[change of origin](https://www.quora.com/What-does-it-mean-when-someone-says-change-of-scale-or-change-of-origin)” across one dimension. A rather trivial transformation.

The transformation looks like:

```
newOrigin = add(oldOrigin, shift);
```

To undo/reverse/invert the transform, you run the same transformation with a negative shift:

```
oldOrigin = add(newOrigin, -shift);
```

In short, the information required to undo ASLR is a single 64-bit integer, which is easy to guess, because if you remember from high-school linear algebra, finding one unknown in a linear equation, requires a single equation (basically a single hint). If you look at each ROP gadget as an “equation” or hint, you notice we have roughly 82 **thousand** of these hints to go by! We only need one!

#### Polymorphism is a super-polynomial problem across multiple dimensions

Polymorphic binaries follow the [fundamental principle of security](https://blog.polyverse.io/how-to-think-about-security-asymmetry-of-difficulty-498eeebe91b5) — they are one-way trivial, while being computationally difficult to reverse.

All Polyverse binaries are at least isomorphs of the root binaries you get. This is what worms and viruses have done for years to bypass anti-viruses. This is why, while WannaCry and Petya were part of the same class of attack, being able to detect one didn’t automatically detect the other. We are finally using the attackers’ methods for good!

Generating an isomorphic graph is relatively easy. It’s not trivial, and definitely not trivial to do at scale, which is what took us so long. The forward transform looks something like this:

```
newGraph = transform(oldGraph, unpredictableEntropySource, time, usableInstructionSubset, usableMemoryAddressSubset, layoutSubset, ...)
```

There’s a bunch of sources of variance and entropy that go into transforming the graph, and we’re adding more each day.

Inverting this transform, however, first requires solving an [incredibly difficult computational problem](https://en.wikipedia.org/wiki/Graph_isomorphism_problem), which only tells you that the two graphs are identical if they were isomorphs.

However, since two execution graphs can be [semantically identical without being isomorphs](https://stackoverflow.com/questions/1132051/is-finding-the-equivalence-of-two-functions-undecidable), they might be completely rewritten in different ways, which sets an attacker up with the second problem — the undecidability problem (you would first have to decide that two nodes across the two graphs are ‘identical’ before the isomorphic comparison would have any meaning.)

This means that an attacker is no longer looking for one 64-bit integer, but trying to guess at an increasing number of variables while up against some of the most difficult computational problems we can think of.

This is why Polymorphic Linux not only breaks the attack, but makes it nearly impossible to adapt it for a particular binary through some clever trick or transform — and even it were possible , it only applies to ONE binary in existence. We produce a different binary per customer multiple times a day.

We just created a nightmare for an attacker, without adding very much complexity for us. We went from a mode of constant evasion, into a mode where we make the attacker have to adapt and solve some very complex problems.

This is why Polymorphic Linux **stops** attacks, instead of *evading* them. When that zero-day comes out, Polymorphic Linux is already…. polymorphic. The defense was already applied. It is not looking for bad instructions, or bad signatures or bad effects. The attack is already defeated on each distinct copy, before we even knew it existed.

#### **Want to try it?**

If this was intriguing, then go ahead and check out our free tier of Polymorphic Linux.

[**Polymorphic Linux® Installation Guide**  
*Install Polymorphic Linux® one step at a time in order to mitigate Linux weaknesses. Depending on your hardware…*polyverse.com](https://polyverse.com/polymorphic-linux-installation-guide/ "https://polyverse.com/polymorphic-linux-installation-guide/")

It’s remarkably simple in how it works. We periodically (daily at present), generate new scrambles across all the packages for Linux distributions — CentOS, Alpine and Ubuntu for now, and we expose the scrambles as drop-in-replacement packages to your package manager.

This means to get a new scramble, all you do is reinstall the package, and a new binary will be delivered. We eat the compute cost of performing the one-way transform.

#### Play with the ROP Analysis tool!

You now have the ROP visualizer tool to study attacks across platforms, and see at a very core level what Polymorphic Linux does.

Since I’m a nerd, I couldn’t just write this post with conceptual block diagrams. I ended up building the tool to really prove the point. The tool though is open source, and one of the easiest ROP gadget finders on the internet right now. You don’t install anything. No platforms. No setups. It runs in your browser and analyzes virtually every major binary in use out there.

I welcome code contributions to it, and go nuts forking it and changing it. Happy hunting!
