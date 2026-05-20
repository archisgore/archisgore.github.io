---
layout: post
title: "Immunize against Spectre!"
description: "Polymorphic Linux makes Spectre attacks nearly impossible"
date: 2018-01-15 07:18:46 +0000
original_url: https://medium.com/@archisgore/immunize-against-spectre-78c53985fb01
---

![](/assets/images/immunize-against-spectre/1_Zmw0UQZ1VHvUCxUllBcMGg.png)
> Spectre depends on binaries being able to be studied and decompiled, to influence how they behave. Polymorphic Linux fundamentally nullifies the entire premise. It’s not even a competition. You can’t influence what you don’t know. Attackers are left confused and baffled.

#### Why Meltdown and Spectre attracted attention

Spectre and Meltdown are unique because:

1. Existing security methodologies can’t stop them. You’ll notice the absence of any antiviruses, firewalls, policy tools, detection agents, scanning tools, and all the usual paraphernalia typically used to treat symptoms after attacks have happened.
2. The attack works across almost every mainstream computer made in the past 20 years.

### How does Spectre work?

This is a highly generalized and simplified explanation of Spectre. A graphical overview of how instructions can be exploited through speculative side-effects is provided in my previous [post on Meltdown](https://blog.polyverse.io/automatic-mitigation-of-meltdown-90f0885130c5).

#### Side-channel data transfer

The foundation of Meltdown and Spectre lies on having the ability to extract data using a side-channel. A side-channel is a measurable/observable side-effect that you can read.

A great example of a side-channel is the cooling fan in my laptop. When I’m reading news online and my fan spins up, it tells me my CPU is overheating. Even though I’m not running any compute-intensive applications. That means one of my browser tabs is probably mining bitcoins at my expense. Thus a simple side-channel can effectively tell me there’s high CPU usage.

#### Influencing the victim to do the attacker’s bidding

The second part of Spectre relies on influencing the victim program to do your bidding, without touching the program itself. If this is done in such a way as to leave a persistent side-effect, it can be read by the attacker program without ever crossing security boundaries.

This relies on having access to the program’s binary, so that it can be studied, decompiled and well understood. How is this possible?

#### Modern computers are miracles of adaptive learning

All the speed gains we’re used to are built on decades of adaptive learning. Nearly everything around you is continually learning and adapting to be better, faster and more efficient. Your internet connection is adapting and remembering what you connect to, and adapting in real time. Your DNS provider is caching your frequently used domains. Many interpreted languages now feature a JITer (Just in Time) compiler — this compiler studies which code paths are most frequently used, and in what ways, so it can rewrite code while it is running, to be more optimal and efficient.

> Anything that can learn can be trained.

This power to learn and adapt can also be used to train and influence. All an attacker need do is train your processor to favor one path above another, and your branch predictor will do the attacker’s bidding in YOUR program.

Even though programs and their data is isolated, the learning is shared.

![](/assets/images/immunize-against-spectre/1_DlQ01DLKhS74oiSvpTfLEQ.png)

The CPU is learning from both. The attack program isn’t doing anything malicious. Even an intelligent scanning tool wouldn’t catch it. Quite the opposite, in fact. It is, however, training the branch predictor that jumping to LP1 results in innocuous behavior! Guess what happens when that branch is predicted in the victim program?

LP1 in the victim program points to dangerous instructions that expose private data (passwords, encryption keys, etc.) to the side-channel. Now, techniques like Meltdown can be used to read memory, EVEN IF the Meltdown patch has been applied.

By telling the CPU repeatedly that LP1 is a safe, innocuous location to jump to, the attacker made the CPU use that learning on the victim program, where that no longer held true.

### Spectre cannot be stopped because it is not a bug

A lot of possible defenses and mitigations are discussed in the paper about Spectre. However the paper makes one big assumption — that all binaries are the same at all times. Compiler-based defenses are assumed to be applicable only once. Obfuscation is assumed to be only applicable on a singular binary — thus severely limiting them.

But here’s the problem — the attack fundamentally works because a binary can be studied and analyzed at leisure. That is where it must be defeated.

Meltdown is technically a “security bug” because the branch speculation works across security zones. Spectre however, is not. Spectre is neither a “bug” nor a “vulnerability.” Spectre fundamentally works because every machine behaves identically. Furthermore, it *relies* on making the machine behave identically.

There is no way to have deterministic identical machine layouts and avoid the natural side-effect — deterministic speculation.

Fundamentally, Spectre is not the cause of anything being “wrong.” This is why tools that look for “wrongness” aren’t working this time. You can’t detect what is correct. You can’t analyze what is correct. AI will only make the speculator learn branches even BETTER. You can’t remedy side-effects that someone might legitimately be relying on.

Spectre is not a vulnerability.

### A credible defense that really works: Polymorphism!

> With polymorphism, adaptive learning continues to work; common learning is defeated.

When we first began investigating this problem of ROP attacks, chains, and memory-based attacks (attacks that rely on knowing the precise contents of programs, layouts and memory), we discussed all the options above, and more.

The reason we went with compile-time isomorphism is precisely to overcome all these limitations in the simplest manner possible.

This is what it looks like in practice:

![](/assets/images/immunize-against-spectre/1_qatZksjzd5dmWTQyaYob5Q.png)

The victim program is not obfuscated. It is an isomorphic equivalent of the original one. But look what happened — and the attacker had no idea! The attacker’s program is still based on the layout they spent time studying. Since our polymorphic program acts and works the same, the attacker has no idea their training is having no effect.

The CPU is still learning from both. It is correctly predicting branches better, giving both programs the performance wins promised. We haven’t stopped or avoided branch prediction. There is nothing wrong with it, it works great and it improves performance.

However, it is no longer learning from the attacker program, and applying those learnings to the victim program.

Polymorphism is a very simple idea, but very effective. This is why Polymorphic Linux PROTECTS machines against Spectre, where other tools have failed. This one diagram explains it all.

#### Want to compare binaries for yourself?

What are these ROP gadgets? What goes in a binary? What do hackers study in binaries? How do they study them?

Don’t worry. We’ve got you covered. First and foremost, you should visit:

[**EnVisen: ROP Entropy Visualizer**  
*ROP Gadget finder/analyser in pure Javascript. Compare gadgets across binaries. Supports most platforms and…*analyze.polyverse.com](https://analyze.polyverse.com "https://analyze.polyverse.com")

For an in-browser open-source binary decompiler.

This tool, as if built specifically to fight Spectre, allows comparisons of two binaries, showing you just how difficult it is to predict or “de-obfuscate” the scrambled binary.

That difficulty is what we call the [**Entropy Quality Index**](https://github.com/polyverse/binary-entropy-visualizer/blob/master/docs/entropy-index.md) or [**Entropy Index**](https://github.com/polyverse/binary-entropy-visualizer/blob/master/docs/entropy-index.md).

Furthermore, check out this series of posts on using the tool to craft real attacks:

[**Let’s craft some real attacks!**  
*If you read security briefings, you wake up every morning to “buffer overflow” vulnerabilities, “control flow” exploits…*blog.polyverse.io](https://blog.polyverse.io/lets-craft-some-real-attacks-8efac7b3df90 "https://blog.polyverse.io/lets-craft-some-real-attacks-8efac7b3df90")

[**Fun with binaries!**  
*ASLR and DEP defeated with three instructions and one offset!*blog.polyverse.io](https://blog.polyverse.io/fun-with-binaries-4361128556a4 "https://blog.polyverse.io/fun-with-binaries-4361128556a4")

[**ASLR simplified!**  
*ASLR explained in one simple picture*blog.polyverse.io](https://blog.polyverse.io/why-aslr-stops-rop-attacks-8e961ca6951e "https://blog.polyverse.io/why-aslr-stops-rop-attacks-8e961ca6951e")

[**How Polymorphic Linux stops attacks when everything else has failed**  
*Using offensive methods against the attacker*blog.polyverse.io](https://blog.polyverse.io/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed-cd8d6b2d669f "https://blog.polyverse.io/how-polymorphic-linux-stops-attacks-when-everything-else-has-failed-cd8d6b2d669f")

And of course, you can try out Polymorphic Linux yourself with our [free trial](https://polyverse.io/polymorphic-linux-installation-guide/).
