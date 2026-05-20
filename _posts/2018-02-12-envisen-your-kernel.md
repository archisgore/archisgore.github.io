---
layout: post
title: "EnVisen your kernel"
description: "Understanding your kernel just became a whole lot easier"
date: 2018-02-12 21:56:06 +0000
original_url: https://medium.com/@archisgore/envisen-your-kernel-3f41198af1ad
---

Studying a kernel’s ROP gadgets is notoriously painful if you don’t do it every day and don’t have the perfect environment ready.

Opening up a [compressed kernel](https://unix.stackexchange.com/questions/197225/is-vmlinuz-and-bzimage-really-the-same) is the first challenge. Linus’s [extraction script](https://github.com/torvalds/linux/blob/master/scripts/extract-vmlinux) is heuristically based and you might need to be on a linux host with all the right tools installed for it to work. When building this, I discovered that the [EFI boot stub](https://wiki.archlinux.org/index.php/EFISTUB) on x86 kernels is a [valid PE file](https://www.linuxquestions.org/questions/linux-general-1/why-is-vmlinuz-shown-as-a-dos-windows-executable-4175493600/) which initially confused my parser.

All of these are problems are in the past. [EnVisen](https://github.com/polyverse/EnVizen) now has the ability to parse compressed kernels, and finding/comparing ROP gadgets in them.

For background, please refer to my series on EnVisen:

- Part 1: <https://blog.polyverse.io/lets-craft-some-real-attacks-8efac7b3df90>
- Part 2: <https://blog.polyverse.io/fun-with-binaries-4361128556a4>
- Part 3: <https://blog.polyverse.io/why-aslr-stops-rop-attacks-8e961ca6951e>
- Part 4: <https://blog.polyverse.io/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed-cd8d6b2d669f>

Let’s give this a whirl…

![](/assets/images/envisen-your-kernel/1_9idODg9wk0Bskm605zrTVw.png)

I used the DataList feature to provide a bunch of samples to be discovered

You’ll notice there is one sample that’s a `vmlinux-3.10.0–693.17.1.el7.x86_64`. As the name indicates this is a straight up uncompressed ELF binary containing the kernel. But that’s too easy. I’ll pick one of the vmlinuz-\* files.

![](/assets/images/envisen-your-kernel/1_LBoUZdWsQYW_1wn7Ae570w.png)

Before I load from the URL, we need to explicitly tell EnVisen to parse this as a compressed kernel image:

![](/assets/images/envisen-your-kernel/1_njk7MUWdduUU07ht0tZuhw.png)

(As a side note, you can totally try NOT selecting vmlinuz, and instead letting it auto-detect format. I was [surprised by what I found](https://www.linuxquestions.org/questions/linux-general-1/why-is-vmlinuz-shown-as-a-dos-windows-executable-4175493600/). At some point I want to rename vmlinuz to .exe and run it on a Windows box just to see my linux kernel execute as a perfectly valid Windows executable.)

Anyway… once we load from the URL, we see:

![](/assets/images/envisen-your-kernel/1_bPdp295YBWWjXwuK8R9Ftw.png)

One thing in particular that I’d like to call out, is the little link to save extracted vmlinux. You can now use EnVisen to extract ELF files out of compressed bootable kernels on any platform and any operating system without having an environment that Linus’s script expects.

If you’re a pentester or security researcher, who wants to look at ROP gadget addresses, and relative offsets, you might find EnVisen a useful tool in your toolbox.
