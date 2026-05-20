---
layout: post
title: "Entropy Quality Index:"
description: "ASLR, KARL, Symbol manipulation, pointer indirection, and more… if it operates on binaries, the Entropy Index determines its effectiveness."
date: 2026-05-20 17:47:48 +0000
---

This is the latest in my long running series on static binary analysis. To recap, I started with [how attacks are crafted](https://blog.polyverse.io/lets-craft-some-real-attacks-8efac7b3df90), then I demonstrated [how attacks can be trivially generated](https://blog.polyverse.io/fun-with-binaries-4361128556a4), then we looked at [how ASLR works](https://blog.polyverse.io/why-aslr-stops-rop-attacks-8e961ca6951e) visually, and finally I covered [what Polymorphic Linux is](https://blog.polyverse.io/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed-cd8d6b2d669f).

A common theme of questions across social media has been, “How is this different; and particularly why does it matter?”

To address that, I started rambling in a document that I called the [Entropy Quality Index](https://github.com/polyverse/binary-entropy-visualizer/blob/master/docs/entropy-index.md). The actual definition is about to change soon as we have a higher quality and more effective measure in the works.

The crux of the document comes down to three key points:

1. A good measure should be based on the point of view of the attacker, i.e., rather than list the thousands of things we did for defense, we must list the things that got in the way of the attacker and what it costs the attacker to overcome our defenses.
2. If an attacker succeeds without having to adapt their attack at all, the EI is zero.
3. If an attacker can never succeed despite modifying their attack in any way possible, the EI is 100.

The definition is built backwards, starting with effectiveness in mind, and then leaving it to the technology to justify itself.

#### Visualize it

Let’s use [EnVisen](https://analyze.polyverse.io) where I plan to maintain the working definition of the Entropy Index at all times.

Just click “Load from URL” on each binary, to populate them:

![](/assets/images/entropy-quality-index/1_RcYk3fTvTguecbU4SpKfPg.png)

Becomes simpler and easier to use each day.

You should be familiar with this:

![](/assets/images/entropy-quality-index/1_bs-W6B4_WeW-eZz8g7nC3w.png)

Woot? Exported symbol offsets!

Now click on the Entropy Analysis button on the right-side binary:

![](/assets/images/entropy-quality-index/1_Mt4Tezq7OoBO_kZSMFlA5Q.png)

At the top is the Entropy Index from Base binary, to the Compared binary.

The pie chart is fairly easy to read, and summarizes how gadgets compare across both binaries (notice it also includes exported symbols.) The definitions of each are in the [Entropy Index definition document](https://github.com/polyverse/binary-entropy-visualizer/blob/master/docs/entropy-index.md).

The histogram needs a bit of work, but you can click “Save offset counts as JSON” to get the raw data behind it. Over time it is intended to be zoomable so you can dig deeper in how gadgets are distributed.
