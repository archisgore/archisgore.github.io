---
layout: post
title: "ASLR simplified!"
description: "ASLR explained in one simple picture"
date: 2017-11-22 19:56:49 +0000
original_url: https://medium.com/@archisgore/why-aslr-stops-rop-attacks-8e961ca6951e
---

**ASLR explained in one simple picture**

ASLR increases [difficulty without adding complexity](https://blog.polyverse.io/how-to-think-about-security-asymmetry-of-difficulty-498eeebe91b5). In [Part 1](https://medium.com/@archisgore/lets-craft-some-real-attacks-8efac7b3df90) and [Part 2](https://blog.polyverse.io/fun-with-binaries-4361128556a4) of this series I demonstrated that crafting attacks can be a pleasant experience without a lot of [furious typing](https://www.youtube.com/watch?v=u8qgehH3kEQ). I’ve even shown you how defeating exploits is easy when we really understand how the attack works. Lets see dive deeper into ASLR, your first line of defense.

![](/assets/images/why-aslr-stops-rop-attacks/1_jyDPtrTrs99NmHfoDcQY3w.png)

Let me explain what you’re seeing in this picture. I loaded a CentOS 7.2 libc-2.17, which we crafted an attack against in my [previous post](https://blog.polyverse.io/fun-with-binaries-4361128556a4). When I loaded the exact same file on the right, I did it with an offset of 16 bytes (10 in hexadecimal).

![](/assets/images/why-aslr-stops-rop-attacks/1_Ay9y2mSy9sK0dHfcBm95Xg.png)

I’m adding features to the tool when I need them for the story.

I picked 16 (hex 10) because it provides easy to interpret uniform offsets across all addresses.

![](/assets/images/why-aslr-stops-rop-attacks/1_djbouRrezPd5-RXLqWN_Og.png)

You’ll notice how the binary on the right is the same binary as on the left, but it’s moved (which is why the lines are all orange.) The gadgets still exist intact but they’re in a different location. Let’s tabulate the first 5 addresses:

```
1807d1 + 0x10 = 1807e1  
1807f1 + 0x10 = 180801  
1807b1 + 0x10 = 1807c1  
1a44cc + 0x10 = 1a44dc  
1770b0 + 0x10 = 1770c0
```

This is clever because, as you saw in the title image, if we tried to execute our ROP chain, `c6169` `c7466` `1b92`, it will work on the original binary, but it falls flat on the offset one.

![](/assets/images/why-aslr-stops-rop-attacks/1_XWz5kE1e6yCuVdqqIXqbMQ.png)

In a nutshell, this is what ASLR does! If we offset the same library differently (and unpredictably) for every program on a machine, the chances that the same attack would work or spread are very low.

Remember, security is not about complexity and two people typing furiously on keyboards, entertaining as that is. Security is about doing what is necessary and sufficient to defeat an attack vector.

#### How is this movement possible?

Offsets are easy because right around the time virtual memory became a thing with the i386, and we moved away from segmented memory to paged memory. All operating systems, processors and compilers came together to work on an offset model. This was originally not intended for security, but rather to enable programs to view a really large memory space, when physically they would only ever use a little bit. It allowed every program to work from memory address 0 through MAX, and the operating system would map it to something real.

ASLR makes use of what already existed which enables any program compiled for a modern operating system to automatically benefit from it.

#### Can we discover more?

I’m particularly proud of this disassembler because you’re not looking at some block diagram I drew in photoshop or name your favorite visualizer program. You’re looking at a real binary of your choice that you uploaded and can now watch these offsets, gadgets and chains at work. This is ASLR on real gadgets in action!

The cliffhanger for this post is to figure out what techniques you might use to discover the offset… remember there’s only one piece of information we need to jump to any ROP location in the offset binary. All I would have to do is add `0x10` to each address in my chain, and I broke ASLR. Like so: `c6179` `c7476` `1ba2`

![](/assets/images/why-aslr-stops-rop-attacks/1_yHgnSseS3O5o3kZ19uY7Xw.png)

This gave me an idea. You’ll notice that somehow `pop rdi ; ret` was in the base library even at the offsetted position! Can we find something common?

I filtered the offsetted library to show surviving gadgets, and some 2,279 gadgets survived.

![](/assets/images/why-aslr-stops-rop-attacks/1_ys4Q_mu8LXHvrwQU8mxJew.png)

I have to admit, I sometimes rig these posts to tell a story but this caught me off guard. I discovered that an offset isn’t enough and a sufficiently LARGE offset is needed if a lot of gadgets tend to occur consecutively. This was crazy!

So the second cliffhanger for today is… given that they ALL offset by a fixed amount, is it possible to infer the offset trivially? The answer is of course yes, since the video in [Part 2](https://blog.polyverse.io/fun-with-binaries-4361128556a4) demonstrated it happening. It’s one thing to read a dry answer and another to intuitively understand it.

Next up I’ll see if I can’t easily figure out an intuitive way to find the offset. I’m basically solving these problems as I write them — this is not some planned series. My team wanted this tool for some other demo, but it ended up being so much fun, I started writing these posts. So I honestly don’t know if I have an answer for intuitive offset-discovery.
