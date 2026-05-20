---
layout: post
title: "Your Linux And Mine Should Be As Different As Mac And Windows"
description: "A blueprint for digital sovereignty at the executable layer, and why countries, banks, datacenters, and embassies should care."
date: 2026-05-12 19:02:56 +0000
original_url: https://medium.com/@archisgore/your-linux-and-mine-should-be-as-different-as-mac-and-windows-637625cf4b89
---

![](/assets/images/your-linux-and-mine-should-be-as-different-as-mac-and-windows/1_JskNjx745Gf6LfakP3x4yw.png)

---

Imagine walking into a Windows shop with a Mac app on a USB stick. You would expect nothing to happen, and nothing would. The app doesn’t run, and we treat this as the natural order of things.

Now imagine walking into a Linux datacenter with a Linux binary on a USB stick. Different company, different country, different threat model. None of it matters. The binary runs, and that should disturb you more than it does.

---

### The monoculture is the bug

Every Linux server you have ever touched — your bank’s, your government’s, your hyperscaler’s, your adversary’s — runs roughly the same Linux as every other Linux server on the planet. Same syscall numbers, same calling convention, same error codes, same `/proc` layout, same ELF format down to the byte. When an attacker writes a working exploit for *a* Linux machine, they have a working exploit for *every* Linux machine, because the economics of an exploit are one-to-many: one bug, ten million reachable targets, frictionless arbitrage.

This is not an accident. POSIX promised it, the GNU project delivered it, distros standardized it, and the cloud immortalized it. *Write once, run everywhere* is a beautiful slogan when you are shipping software and a catastrophe when you are defending it.

---

### Where Polymorphic Linux stopped

I spent eight years at Polyverse building [Polymorphic Linux](https://blog.polyverse.io/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed-cd8d6b2d669f) — drop-in scrambled binaries for the major distros, so the ROP gadgets in your `libc` were not the gadgets in mine. Memory-corruption exploits broke, and we shipped to hospitals, telecoms, and governments. The math was right, but the boundary was wrong.

Polyverse scrambled the *contents* of binaries without touching the *contract* between them. Two Polymorphic Linux machines still spoke the same Linux: same syscall numbers, same ABI, same everything an attacker needs to construct the next exploit. We diversified the gadgets while leaving the grammar intact.

[**encrypted-linux**](https://github.com/encrypted-execution/encrypted-linux) finishes the job.

---

### A Linux with a private dialect

It is a Linux distribution in which the grammar itself is per-build. The kernel, the C library, the compiler, and every binary produced by that toolchain agree on a private dialect derived from a single seed, such that a different seed yields a different dialect in its entirety.

Pick a seed and build your Linux. Your `write()` is no longer syscall number 1 but a 64-bit value nobody else holds. Your function calls do not pass arguments in `%rdi` but wherever your seed dictates. Your `ENOENT` is not 2. Your `/proc/[pid]/status` carries no field named `VmRSS`. Your binaries carry an `EI_OSABI` byte meaning *this build, this seed*, and anything stamped differently — a stock Ubuntu binary, a leaked attacker dropper, an off-the-shelf rootkit — receives `ENOEXEC` from the kernel before it executes a single instruction.

This is not obfuscation, not a sandbox, not a watermark. It is a language barrier. Your Linux and mine are now as different as Mac and Windows, not because we ran them through different obfuscators, but because they speak different machine-level languages at every layer of the contract. The same exploit cannot work on both. The same shell-script payload, the same gadget chain, the same kernel rootkit — none of it ports. Attackers who once wrote once and owned everywhere must now write against each target independently, per build, per country, per company, per rack.

---

### Execution deterrence

I call this **execution deterrence**, and the promise it makes is narrower and more honest than the one most security products offer. It is not “you cannot break in,” which is a promise nobody can keep. It is this:

> If you do break in, the code you brought with you doesn’t run.

The cost of an exploit collapses from one-to-many to one-to-one. We reintroduce friction into a market the attackers had perfectly arbitraged away, converting a free attack into a paid one and a one-to-billions broadcast into billions of separate one-to-ones.

---

### Whose Linux?

This is, at its foundation, a digital sovereignty story.

Sovereignty is not only about where your data lives. It is about whether the executable layer of your civilization belongs to you. At present, the answer is no. The executable layer is a shared global commons, and every nation, every bank, every embassy, every hyperscaler is downstream of the same Linux ABI that has been frozen since the 1990s. When CVE-2026-whatever drops, your sovereign-cloud nameplate offers no protection, because the same exploit works against your air-gapped agency, your enemy’s grid, and your neighbor’s bank with equal indifference.

Consider what changes if that were no longer true.

**A country** builds its sovereign Linux, so that its weather agency, its tax service, and its strategic command all speak the same dialect, built nowhere else. Other countries’ attack toolkits do not compile against it, and CVE-class exploits stop crossing borders.

**A bank** builds its Linux per business unit and rotates quarterly. Trading runs one dialect, retail another, and the SWIFT gateway a third. An exploit that lands in one segment is a paperweight in the others, because lateral movement has become a translation problem rather than a path-finding problem.

**A hyperscaler** builds a fresh Linux per tenant, making two customers on the same physical hardware as foreign to each other as Mac and Windows. Cross-tenant escape stops being an exercise in finding the right syscall hole and becomes an exercise in guessing 64 bits of calling convention, error codes, and procfs schema simultaneously.

**An embassy** carries its own Linux on its own drive. The host country’s surveillance toolkit, built against everyone’s Linux, finds itself fluent in the wrong language, and the classic bag-search attack stops working.

None of this is slideware. The kernel patches, the compiler patches, the libc patches, and the build orchestrator are all in the [repo](https://github.com/encrypted-execution/encrypted-linux). The reference image boots in QEMU. The `hello` binary runs. The stock-Ubuntu `hello` does not. The CI badge stays green.

---

### “But isn’t the seed recoverable?”

This is the crypto-nitpicker’s question, and while it is the right question, it is being applied to the wrong problem.

The seed is public in precisely the same sense that your house key is public — you carry it in your pocket every day, and in principle anyone could obtain it. The security does not rest on the impossibility of that acquisition. It rests on the fact that acquisition costs something, that the cost is per-target, and that the next target carries the same cost all over again. Strong cryptography raises the per-target cost, which is desirable, but the deterrent does not require unbreakable crypto. It requires any friction at all, applied uniformly to every contract in the stack, scaled by however many independent dialects you choose to maintain. That number is your deterrence dial, and you set it wherever your threat model demands.

---

### You cannot patch your way to security

Roger Schell, the father of trusted computing, wrote in 1979 that you cannot patch your way to security. He was describing the bolt-on industry that would consume the next forty-five years of the discipline, and we have, right on schedule, vindicated his pessimism. We have audited, scanned, EDR’d, ML’d, and Zero-Trusted ourselves into a position where the same exploit still ports across continents without modification.

The question worth asking is whether the cure is more detection at all, or whether it is a thinner shared surface. Encrypted-linux is one answer to that question. The [Encrypted Execution paper](https://www.encrypted-execution.com/), whose patent I [pledged to the public domain](https://medium.com/@archisgore/pledging-encrypted-execution-patent-to-public-domain-3a8a2bfde1f6), makes the broader case: per-build cryptographic diversification at every layer where a contract exists, making the contract private and the language yours.

---

### The one thing to take away

You need not believe the cryptography is unbreakable, nor that the seed is invisible, nor that encrypted-linux is the final shape of the idea. You need only accept a single premise:

> If your Linux is the same as your adversary’s Linux, you are sharing the executable layer with your adversary. And if you are sharing the executable layer with your adversary, you are not sovereign.

Solving that problem requires no additional firewalls, no additional SOC analysts, no additional AI. It requires a build process that produces a Linux belonging to you alone. The code is on [GitHub](https://github.com/encrypted-execution/encrypted-linux), Apache-2.0, patent pledged. Make a Linux nobody else has, one that runs nobody else’s code — make a Linux that is yours.
