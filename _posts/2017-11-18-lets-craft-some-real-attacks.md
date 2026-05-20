---
layout: post
title: "Let’s craft some real attacks!"
description: "If you read security briefings, you wake up every morning to “buffer overflow” vulnerabilities, “control flow” exploits, crafted attacks…"
date: 2017-11-18 05:47:01 +0000
original_url: https://medium.com/@archisgore/lets-craft-some-real-attacks-8efac7b3df90
---

If you read security briefings, you wake up every morning to “buffer overflow” vulnerabilities, “control flow” exploits, crafted attacks against specific versions of code, and whatnot.

Most of those descriptions are bland and dry. Moreover, much of it makes no intuitive sense, everyone has their fad of the week, and it is easy to feel disillusioned. What’s real, and what’s techno-babble? Didn’t we just pay for the firewalls and deploy the endless stream of patches? What is with all this machine-code nonsense?

A gripe I’ve always had with our industry is that the first solutions we come up with are architectural ivory towers. We try curing cancer on day one, and then in a few years we would sell our soul just to be able to add two numbers reliably. (Yeah, I’m still holding a grudge against UML, CORBA, SOAP, WSDL, and oh for god’s sake — DTDs!)

Let’s skip all that and actually begin by crafting a real attack visually and interactively! No more concepts. No more theory. No more descriptions of instruction set layouts and stacks and heaps! Liberal screenshots to follow! Brace yourself! This is as colorful as binaries will ever get!

### Let’s play attacker for a bit

#### Intro to Tools

Let’s start by visiting this tool I wrote specifically for this blog post, and open a binary.

<https://analyze.polyverse.com>

(Source code here: <https://github.com/Polyverse/binary-entropy-visualizer>)

![](/assets/images/lets-craft-some-real-attacks/1_aWepyyXfYDhD37qVgoQqrw.png)

Everytime I build a web app, I end up putting a CLI in there.

Now you can drag-drop a file on there to analyze it — yeah that web page is going to do what advanced geeky nerdy tools are supposed to do on your desktop. For now it only supports Linux 64-bit binaries. Don’t look too hard, there’s two samples provided on my github repo: <https://github.com/polyverse/binary-entropy-visualizer/tree/master/samples>. Simply download either of the files ending in “.so”.

When you throw it on there, it should show you a progress bar with some analysis…..

![](/assets/images/lets-craft-some-real-attacks/1_CegDhM5RWE4hpn3z-q_yZg.png)

Getting this screenshot was hard — it analyzes quickly.

If you want to know what it’s doing, click on the progress bar to see a complete log of actions taken.

![](/assets/images/lets-craft-some-real-attacks/1_bdfL2Gw0WIJrIRQHU5pWtQ.png)

Proof: Despite my best attempts, I hid a CLI in there for myself.

When analysis is complete, you should see a table. This is a table of “[ROP gadgets](https://en.wikipedia.org/wiki/Return-oriented_programming).” You’re witnessing a live analysis in your browser of what people with six screens in dark rooms run with complex command lines and special programs.

![](/assets/images/lets-craft-some-real-attacks/1_VFfHVqkKbRsDJJaeNYS2UQ.png)

But wait.. what about those other two sections?

We won’t go into what ROP gadgets are, what makes them a gadget and so on. Anyone who’s ever gone through Programming 101 will recognize it as “[Assembly Language code](https://en.wikipedia.org/wiki/Assembly_language)”, another really fun thing that is always presented as dry and irritating. It’s also everywhere.

#### What is an exploit?

> Execution of unwanted instructions

In the fashion of my patron saints, McGyver (the old one) and the Mythbusters, I am not going to go into how you find a buffer overrun and get to inject stuff onto a stack and so on. Sorry. Plenty of classes online to learn how to do that, or you might want to visit Defcon.

Let’s just assume you have a process with a single byte buffer overrun. This isn’t as uncommon as you’d think. Off-by-one errors are plentiful out there. Sure, everyone should use [Rust](https://www.rust-lang.org/en-US/), but didn’t I just rant about how we all want to be “clever” and struggle to plug holes later?

Let’s simply accept that an “exploit” is a set of commands you send to a computer to do what you (the attacker) wants, but something the owner/developer/administrator (the victim) definitely does not want. No matter what name the exploit goes under, at the end of the day it comes down to executing instructions that the attacker wants, and the victim doesn’t. What does stealing/breaking a password do? Allow execution. What does a virus do? Executes instructions. What does SQL-injection do? Executes SQL instructions.

Remember this: execution of unwanted instructions is bad.

#### Always know what you want

> We want a specific set of instructions to run, given below.

Okay let’s craft an exploit now. We’re going to simulate it. All within the browser.

Let’s say for absolutely arbitrary reasons that running the following instructions makes something bad happen. WOPR starts playing a game. Trust me: nobody wants that! You don’t have to understand assembly code. In your mind, the following should translate to, “[Later. Let’s play Global Thermonuclear War.](https://www.youtube.com/watch?v=ecPeSmF_ikc)”

`jbe 0x46c18 ; nop ; mov rax, rsi ; pop rbx ;  
add byte ptr [rax], al ; add bl, ch ; cmpsb byte ptr [rsi], byte ptr [rdi] ; call rax  
and al, 0xf8 ;   
mov ecx, edx ; cmp rdx, rcx ; je 0x12cb78 ;   
jne 0x1668e0 ; add rsp, 8 ; pop rbx ; pop rbp ;   
sub bl, bh ; jmp qword ptr [rax]  
add byte ptr [r8–0x77], r9b ; fimul dword ptr [rax — 0x77] ;   
or byte ptr [rdi], 0x94 ;   
push r15 ; in eax, dx ; jmp qword ptr [rdx]  
jg 0x95257 ; jne 0x95828 ;   
jb 0x146d9a ; movaps xmmword ptr [rdi], xmm4 ; jmp r9  
or byte ptr [rdx], al ; add ah, dl ; div dh ; call rsp  
jg 0x97acb ; movdqu xmmword ptr [rdi + 0x10], xmm2 ;   
or dword ptr [rax], eax ; add byte ptr [rax], al ; add byte ptr [rax], al ;   
add byte ptr [rax], al ; enter 8, 0 ;   
xor ch, ch ; mov byte ptr [rdi + 0x1a], ch ;`

So how do we do it? The most effective ways to do this is to use social engineering, spearfishing, password-guessing, etc, etc. They are also ways that leave traces. They are effective and blunt, and, with enough data, they will be caught. Also, look at that code. Once someone figures out that this set of instructions causes bad things, it is easy to generate a signature to find any bits of code that match it, and prevent it from running.

But I wouldn’t be writing this post if that was the end of it.

Just because you can’t inject this code through the other methods, doesn’t mean you can’t inject code that will cause this series of instructions to be executed. AI/analytics/machine learning: all suffer from one big flaw — the Turing Test.

A program isn’t malicious because it “has bad instructions.” There’s no such thing as “bad instructions”. Why would processors, and machines and servers and phones ship with “bad instructions?” No, *there are bad sequences of instructions*!

A program doesn’t necessarily have to carry the bad sequence within itself. All it has to do is carry friendly good sequences, which, on the target host, lead to bad sequences getting executed. If you haven’t guessed already, this behavior may not necessarily be malicious; it might even be accidental.

#### How to get what you want

Now go back to the tool if you haven’t closed it. Use the file “[libc-2.17.so](https://github.com/polyverse/binary-entropy-visualizer/blob/master/samples/centos/libc-2.17.so)” from the samples, and load it.

Then enter this sequence of numbers in the little text box below “ROP Chain Execution:”

`46c1c 7ac3f 46947 12cb5f 166900 183139 cfdcb 12f7ea 191614 95236 146d8a 1889ad 97abb 4392 17390e 98878`

It should look something like this:

![](/assets/images/lets-craft-some-real-attacks/1_YDW-uFx-A0kqCUrKFVZymA.png)

Go ahead and execute the chain.

![](/assets/images/lets-craft-some-real-attacks/1_7-jlzQ-VphnzAUuTnjLEfA.png)

Well guess what? An exact match to my instructions to activate WOPR!

The libc that you just analyzed is a fundamental and foundational library linked into practically any and every program on a Linux host. It is checked and validated and patched. Each of those instructions is a good instruction — approved and validated by the processor-maker, the compiler-maker, the package manager all the way down to your system administrator.

#### What’s a REAL sequence of bad instructions?

> pop rdi, pop rsi, pop rdx, and offset of [mprotect](http://www.informit.com/articles/article.aspx?p=23618&seqNum=10) is all it takes!

I made up the sequence above. In a complete break from convention, I made it *more* complex just so it’d look cool. Real exploits require gadgets so simple, you’ll think I’m making this part up!

A real known dangerous exploit we (simulated) in our lab requires only three ROP gadget locations, and the offset to [mprotect](http://www.informit.com/articles/article.aspx?p=23618&seqNum=10) within libc. We can defeat ASLR remotely in seconds, and once we call mprotect, we can make anything executable that we want.

You can see how easy it is to “Find Gadget” and create your own chain for:  
`pop rdi ; ret  
pop rsi ; ret  
pop rdx ; ret`

This illustrates how simple exploits hide behind cumbersome tools, giving the illusion of difficulty or complexity.

#### Crafting your own real, serious payloads

So why is this ROP analyzer such a big deal? If you haven’t put two and two together, an exploit typically works like this:

1. You figure out what you want (we covered this step above).
2. You need to figure out a sequence of instruction groups, all ending with some kind of a jump/return/call that you can exploit to get the intermediate instructions in between executed.

Turns out that step 2 is not so easy. You need to know what groups of instructions you have to play with, so you can craft chains of them together.

This tool exports these little instruction-groups (called gadgets) from the binaries you feed it. You can then solve for finding which gadgets in what sequence will get achieve your goal.

This is a complex computational problem that I won’t solve today.

Look out for [Part 2 of my post](https://medium.com/@archisgore/fun-with-binaries-4361128556a4) which will go into what the other “Compare File” dialog is for… stay tuned! It’s dead trivial to figure out, anyway, so go do it if you want.
