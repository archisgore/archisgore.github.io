---
layout: post
title: "How Meltdown works — visualized"
description: "Let’s look at what Meltdown is and how it works, as well as how it is stopped. A lot has been written about the Meltdown vulnerability, but…"
date: 2018-01-15 07:18:30 +0000
original_url: https://medium.com/@archisgore/automatic-mitigation-of-meltdown-90f0885130c5
---

Let’s look at what [Meltdown](https://meltdownattack.com/meltdown.pdf) is and how it works, as well as how it is stopped. A lot has been written about the Meltdown vulnerability, but it is still commonly misunderstood. A few diagrams may help.

First, let’s consider a simplified memory hierarchy for a computer: main memory, split into user memory and kernel memory; the cache (typically on the CPU chip); and then the CPU itself.

![](/assets/images/automatic-mitigation-of-meltdown/1_c2pzh7ibGUlv7Huqr72D2Q.png)

The bug is pretty simple. For about two decades now, processors have had a flag that tells them what privilege level a certain instruction is running in. If an instruction in user space tries to access memory in kernel space (where all the important stuff resides), the processor will throw an exception, and all will be well.

On certain processors though, the speculative executor fails to check this bit, thus causing side-effects in user space (caching of a page), which the user space instructions can test for. The attack is both clever and remarkably simple.

Let’s walk through it graphically. Assume your memory starts with this flushed cache state — nothing sits in the cache right now (the “flush” part of what is a a “flush-reload” attack):

![](/assets/images/automatic-mitigation-of-meltdown/1_cNEK3iAPmOm0KEHy3y8Eaw.png)

#### Step 1: Find cached pages

First let’s allocate 256 pages on the user space that we can access. Assuming a page size of 4K, we just allocate 256 times 4K bytes of memory. It doesn’t matter where those pages reside in user-space memory, so long as we got the page size correct. In C-style pseudo-code:

```
char userspace[256 * 4096];
```

I’ll mark those in the userspace diagram — for brevity, I’ll only show a few pages, and I’m going to show cached pages popped up like this:

![](/assets/images/automatic-mitigation-of-meltdown/1_CmsQFicz5hs29ZIBpoQUwQ.png)

This allows for easier reading (and easier drawing for me!).

So let’s start with an empty (flushed) cache:

![](/assets/images/automatic-mitigation-of-meltdown/1_htikQdT4J_gwTgSlOerb0g.png)

We know what the cache state would be if we accessed a byte in page 10. Since any byte in page 10 would do the trick, let’s just use the very first byte (at location 0).

The following code accesses that byte:

```
char dummy = userspace[10 * 4096];
```

This leads the state to be:

![](/assets/images/automatic-mitigation-of-meltdown/1_fgr8k-KbUb0nsCEG3Zi2bA.png)

Now what if we measured the time to access each page and stored it?

```
int accessTimes[256];
```

```
for (int i=0; i < 256; i++) {
```

```
    t1 = now();  
    char dummy = userspace[i * 4096];
```

```
    t2 = now();  
    accessTimes[i] = t2-t1;
```

```
}
```

Since page 10 was cached, page 10’s access time would be [significantly faster](https://www.extremetech.com/extreme/188776-how-l1-and-l2-cpu-caches-work-and-why-theyre-an-essential-part-of-modern-chips) than all other pages which need a roundtrip to main memory. Our access times array would look something like this:

```
accessTimes = [100, 100, 100, 100, 100, 100, 100, 100, 100, 10, 100, 100....];
```

The 10th value (page 10) is an order of magnitude faster to access than anything else. So page 10 is cached, whereas others were not. Note though that all of the pages did get cached as part of this access loop. This is the “reload” part of the flush-reload side-channel — because we reloaded all pages into the cache.

At this point we can figure out which pages are cached with ease if we flush the cache, allow someone else to affect it, then reload it.

#### Step 2: Speculate on kernel memory

This step is easy. Let’s assume we have a pointer to kernel memory:

```
char *kernel = 0x1000; //or whatever the case is
```

If we tried to access it using an unprivileged instruction, it would fail — our user space instructions don’t have a privileged bit set:

```
char important = kernel[10];
```

Speculating this is easy. The instruction above would speculate just fine. It would then throw an exception, which would cause us to never get the value of **important**.

#### Step 3: Affect userspace based on speculated value

However, what happens if we speculated this?

```
char dummy = userspace[kernel[10] * 4096]
```

We know userspace has 256 \* 4096 bytes — we allocated it. Since we’re only reading one byte from the kernel address, the maximum value is 255.

What happens when this line is speculated? Even though the processor detected the segmentation fault and prevented you from reading the value, did you notice that it cached the user-space page? The page whose number was the value of kernel memory!

Suppose the value of`kernel[10]` was 17. Let’s run through this:

1. Processor obtained `kernel[10]` using the branch predictor. That value was `17`.
2. The processor then dereferenced the 17th 4K-wide page in the array “userspace”: `userspace[17 * 4096]`
3. The processor detected that you weren’t allowed to access kernel[10], and so told you you can’t execute the branch. Bad programmer!
4. The processor left the cache untouched. It’s not going to let you touch kernel memory on the cache though. It’s got your back…

What was the state of cache at the end of this?

![](/assets/images/automatic-mitigation-of-meltdown/1_TASc8Vhju7Utz0de8TFvcg.png)

That’s cool! Using Step 1, we would get the 17th page time being the fastest — by a large amount from the others! That tells us the value of `kernel[10]` was 17, even though we never accessed `kernel[10]`!

Pretty neat huh? By going over the kernel byte by byte, we can get the value of every kernel address, by affecting cache pages.

### What went wrong? How are we fixing it?

Meltdown is a genuine “bug” — it’s not in the side-channel. The bug is straightforward — CPU speculative execution should not cross security boundaries — and ultimately should be fixed in the CPU itself.

It’s not the cache that’s misbehaving — even though that’s where most operating-system vendors are fixing it. More precisely, they are attempting to further isolate kernel and userspace memory, using something called Kernel Page Table Isolation (KPTI), previously called KAISER. It maps very few “stub” pages to the process’s virtual memory, keeping the kernel out (and thus not reachable by the speculative execution engine).

![](/assets/images/automatic-mitigation-of-meltdown/1_EjvOrGo8wpjdAXJtQfqrPA.png)

Unfortunately, this segmentation is coming at a [cost](https://newsroom.intel.com/editorials/intel-security-issue-update-initial-performance-data-results-client-systems/) — accessing kernel memory now requires more expensive hardware-assisted transitions.

#### Polymorphic Linux stops ROP attacks; increases difficulty of others

Since Polymorphic Linux was intended for stopping ROP attacks dead in their tracks, all ROP attacks in kernel space are defeated by using polymorphic kernels. Especially when KASLR (kernel address space layout randomization) is defeated (which is so trivial that the Meltdown paper leaves it as an exercise for the reader).

Furthermore, since polymorphic binaries have different signatures, layouts, instructions and gadgets, they make it difficult by at least an order of magnitude to craft further attacks. Polymorphic binaries force the extra step of analysis and understanding per binary. This means that a lateral attack (one that moves from machine to machine in a network) becomes much harder.

Look out for my next [post on Spectre](https://blog.polyverse.io/immunize-against-spectre-78c53985fb01). It’s a bit more difficult to explain and definitely harder than Meltdown to craft…
