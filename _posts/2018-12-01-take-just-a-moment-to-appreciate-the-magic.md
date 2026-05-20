---
layout: post
title: "Take just a moment to appreciate the magic"
description: "that makes every machine around us work…"
date: 2018-12-01 00:34:04 +0000
original_url: https://medium.com/@archisgore/take-just-a-moment-to-appreciate-the-magic-c8f31d308015
---

As is usual at Polyverse, I was writing some unit tests to ensure the disassembler was doing the right thing.

Observe something wonderful in the disassembly below: the gadget sequence is interpreted differently, depending on where you begin parsing.

```
0x000000000029: RET
```

```
0x000000000028: POP RBP; RET
```

```
0x000000000027: POP RDI; POP RBP; RET
```

```
0x000000000026: POP R15; POP RBP; RET
```

```
0x000000000025: POP RSI; POP R15; POP RBP; RET
```

```
0x000000000024: POP R14; POP R15; POP RBP; RET
```

```
0x000000000023: POP RBP; POP R14; POP R15; POP RBP; RET
```

```
0x000000000022: POP R13; POP R14; POP R15; POP RBP; RET
```

```
0x000000000021: POP RSP; POP R13; POP R14; POP R15; POP RBP; RET
```

```
0x000000000020: POP R12; POP R13; POP R14; POP R15; POP RBP; RET
```

Look at the lines I’ve highlighted. From address 26, the instruction is `POP R15`, but from 27, it is `POP RDI`. What is interesting is that they are both valid instructions. The same thing happens in each pair of gadgets following it.

Why’s this so cool? For one, it represents the challenge we are up against when we perform scrambling. We have to get EVERY SINGLE BYTE right. One single byte off, and the entire meaning of the program changes.

But secondly, just appreciate how cool it is that everything on your browser, my browser, the medium servers and the internet is working in a perfectly synchronous elegant… ballet. Every single byte is PRECISELY being interpreted by every processor, memory bank, network packet and storage device. From the keyboard, to the mouse, to device drivers, across multiple cores, multiple browser tabs. There’s something worth stepping back and appreciating in how beautifully that is.

Here’s to all you processor designers, silicon builders, OS writers, driver-writers and many more — who make all this work:

Thank you!
