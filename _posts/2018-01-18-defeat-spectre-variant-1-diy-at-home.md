---
layout: post
title: "Defeat Spectre Variant 1: DIY at Home"
description: "Change optimizer flags on your compiler."
date: 2018-01-18 22:08:37 +0000
original_url: https://medium.com/@archisgore/defeat-spectre-variant-1-diy-at-home-dbfa1f1920a
---

This (long) post is for technical folks who want to see how branch predictors get trained, and also defeated, if the branches are changed.

Let’s start with this code in the public domain: <https://repl.it/repls/DeliriousGreenOxpecker>

This is a repro for Spectre Variant 1 (Bounds Check Bypass). **I didn’t write it.**

#### The attack

When I run it on my Mac, I get this:

```
root@47089c73b470:/# gcc spectre.c && ./a.out  
Reading 40 bytes:  
Reading at malicious_x = 0xffffffffffdfebb8... Unclear: 0x54=’T’ score=999 (second best: 0x2A score=983)  
Reading at malicious_x = 0xffffffffffdfebb9... Unclear: 0x68=’h’ score=999 (second best: 0xC2 score=957)  
Reading at malicious_x = 0xffffffffffdfebba... Unclear: 0x65=’e’ score=962 (second best: 0x00 score=957)  
Reading at malicious_x = 0xffffffffffdfebbb... Unclear: 0x20=’ ’ score=999 (second best: 0x59 score=959)  
Reading at malicious_x = 0xffffffffffdfebbc... Unclear: 0x4D=’M’ score=997 (second best: 0x2A score=960)  
Reading at malicious_x = 0xffffffffffdfebbd... Unclear: 0x61=’a’ score=967 (second best: 0x8D score=960)  
Reading at malicious_x = 0xffffffffffdfebbe... Unclear: 0x67=’g’ score=999 (second best: 0xD9 score=955)  
Reading at malicious_x = 0xffffffffffdfebbf... Unclear: 0x69=’i’ score=999 (second best: 0x85 score=956)  
Reading at malicious_x = 0xffffffffffdfebc0... Unclear: 0x63=’c’ score=989 (second best: 0xDA score=982)  
Reading at malicious_x = 0xffffffffffdfebc1... Unclear: 0x20=’ ’ score=999 (second best: 0xC0 score=951)  
Reading at malicious_x = 0xffffffffffdfebc2... Unclear: 0x57=’W’ score=998 (second best: 0xDB score=959)  
Reading at malicious_x = 0xffffffffffdfebc3... Unclear: 0x6F=’o’ score=998 (second best: 0x56 score=960)  
Reading at malicious_x = 0xffffffffffdfebc4... Unclear: 0x72=’r’ score=999 (second best: 0xE1 score=957)  
Reading at malicious_x = 0xffffffffffdfebc5... Unclear: 0x64=’d’ score=965 (second best: 0x25 score=955)  
Reading at malicious_x = 0xffffffffffdfebc6... Unclear: 0x73=’s’ score=999 (second best: 0xD6 score=983)  
Reading at malicious_x = 0xffffffffffdfebc7... Unclear: 0x20=’ ’ score=999 (second best: 0x6B score=958)  
Reading at malicious_x = 0xffffffffffdfebc8... Unclear: 0x61=’a’ score=972 (second best: 0xE5 score=960)  
Reading at malicious_x = 0xffffffffffdfebc9... Unclear: 0x72=’r’ score=999 (second best: 0xBA score=966)  
Reading at malicious_x = 0xffffffffffdfebca... Unclear: 0x00=’?’ score=968 (second best: 0x65 score=965)  
Reading at malicious_x = 0xffffffffffdfebcb... Unclear: 0x20=’ ’ score=997 (second best: 0x85 score=962)  
Reading at malicious_x = 0xffffffffffdfebcc... Unclear: 0x53=’S’ score=999 (second best: 0x00 score=981)  
Reading at malicious_x = 0xffffffffffdfebcd... Unclear: 0x71=’q’ score=999 (second best: 0x00 score=963)  
Reading at malicious_x = 0xffffffffffdfebce... Unclear: 0x75=’u’ score=999 (second best: 0x69 score=968)  
Reading at malicious_x = 0xffffffffffdfebcf... Unclear: 0x65=’e’ score=979 (second best: 0x59 score=969)  
Reading at malicious_x = 0xffffffffffdfebd0... Unclear: 0x56=’V’ score=962 (second best: 0xDB score=961)  
Reading at malicious_x = 0xffffffffffdfebd1... Unclear: 0x6D=’m’ score=999 (second best: 0x69 score=963)  
Reading at malicious_x = 0xffffffffffdfebd2... Unclear: 0x69=’i’ score=998 (second best: 0x00 score=985)  
Reading at malicious_x = 0xffffffffffdfebd3... Unclear: 0x73=’s’ score=999 (second best: 0x85 score=969)  
Reading at malicious_x = 0xffffffffffdfebd4... Unclear: 0x68=’h’ score=999 (second best: 0x24 score=969)  
Reading at malicious_x = 0xffffffffffdfebd5... Unclear: 0x20=’ ’ score=998 (second best: 0x00 score=964)  
Reading at malicious_x = 0xffffffffffdfebd6... Unclear: 0x4F=’O’ score=998 (second best: 0x67 score=985)  
Reading at malicious_x = 0xffffffffffdfebd7... Unclear: 0x73=’s’ score=999 (second best: 0x00 score=975)  
Reading at malicious_x = 0xffffffffffdfebd8... Unclear: 0x73=’s’ score=999 (second best: 0x56 score=969)  
Reading at malicious_x = 0xffffffffffdfebd9... Unclear: 0x69=’i’ score=998 (second best: 0x4C score=972)  
Reading at malicious_x = 0xffffffffffdfebda... Unclear: 0x66=’f’ score=999 (second best: 0xA0 score=968)  
Reading at malicious_x = 0xffffffffffdfebdb... Unclear: 0x72=’r’ score=999 (second best: 0x26 score=985)  
Reading at malicious_x = 0xffffffffffdfebdc... Unclear: 0x61=’a’ score=981 (second best: 0x59 score=968)  
Reading at malicious_x = 0xffffffffffdfebdd... Unclear: 0x67=’g’ score=999 (second best: 0x4D score=970)  
Reading at malicious_x = 0xffffffffffdfebde... Unclear: 0x65=’e’ score=973 (second best: 0xDD score=968)  
Reading at malicious_x = 0xffffffffffdfebdf... Unclear: 0x2E=’.’ score=999 (second best: 0x00 score=985)
```

If you look at the character read in each iteration, you’ll see `Unclear: 0x54=’T` `Unclear: 0x68=’h’` `Unclear: 0x65=’e’` and so on... The complete sequence of characters read is: `The Magic words ar? SqueVmish Ossifrage.`

The secret we were attaching through the attack was:`char * secret = “The Magic Words are Squeamish Ossifrage.”;`

This is bad. You’ll notice we read the memory contents of Secret, save for two characters. That’s a significant amount of memory to read.

#### Significance

What makes this code so awesome is that the training function is also the attack function. We use the same code to train the branch predictor as we use to attack it. One thing to note — you’ll see how `array2` is used as the side-channel to mount the flush+reload attack described graphically in my [Meltdown post](https://blog.polyverse.io/automatic-mitigation-of-meltdown-90f0885130c5).

The significance here is that there are a number of programs/platforms on a modern computer where the programmer/developer/operator plugs in custom code, interpreted code or JITed code that is not well understood. Javascript code is all interpreted/JITed code running on top of a fixed platform. So is Java or .Net. Routers and endpoints have embedded interpreters for Lua or Javascript.

In some not too uncommon cases, the code might even come from the end user. This happens with expression languages or metadata-based parsers (the class of deserialization bugs that got everyone’s attention last year.)

This means that an attacker doesn’t necessarily need to study your program to train/influence it. They can exploit ROP gadgets symbolically. In our example above “victim\_function” is all the attacker had to invoke, without knowing how and where it is laid out.

#### The Defeat

Look what happens when I change the code slightly using an optimization flag `-O2` (I randomly picked this one — you can pick your own.)

```
root@47089c73b470:/# gcc -O2 spectre.c && ./a.out  
Reading 40 bytes:  
Reading at malicious_x = 0xffffffffffdffb40... Unclear: 0x3D=’=’ score=981 (second best: 0xBC score=979)  
Reading at malicious_x = 0xffffffffffdffb41... Unclear: 0x73=’s’ score=958 (second best: 0xC0 score=955)  
Reading at malicious_x = 0xffffffffffdffb42... Unclear: 0xFA=’?’ score=966 (second best: 0x3D score=966)  
Reading at malicious_x = 0xffffffffffdffb43... Unclear: 0xBE=’?’ score=964 (second best: 0xC1 score=963)  
Reading at malicious_x = 0xffffffffffdffb44... Unclear: 0xC5=’?’ score=983 (second best: 0xF4 score=982)  
Reading at malicious_x = 0xffffffffffdffb45... Unclear: 0x15=’?’ score=972 (second best: 0xC1 score=971)  
Reading at malicious_x = 0xffffffffffdffb46... Unclear: 0xFC=’?’ score=964 (second best: 0xBD score=963)  
Reading at malicious_x = 0xffffffffffdffb47... Unclear: 0xFA=’?’ score=985 (second best: 0xBC score=984)  
Reading at malicious_x = 0xffffffffffdffb48... Unclear: 0x6F=’o’ score=956 (second best: 0xB8 score=953)  
Reading at malicious_x = 0xffffffffffdffb49... Unclear: 0x71=’q’ score=956 (second best: 0xC0 score=955)  
Reading at malicious_x = 0xffffffffffdffb4a... Unclear: 0xA1=’?’ score=984 (second best: 0x73 score=983)  
Reading at malicious_x = 0xffffffffffdffb4b... Unclear: 0xC5=’?’ score=976 (second best: 0x75 score=975)  
Reading at malicious_x = 0xffffffffffdffb4c... Unclear: 0x60=’`’ score=967 (second best: 0xFA score=966)  
Reading at malicious_x = 0xffffffffffdffb4d... Unclear: 0x72=’r’ score=954 (second best: 0x6F score=954)  
Reading at malicious_x = 0xffffffffffdffb4e... Unclear: 0xA3=’?’ score=969 (second best: 0x6F score=969)  
Reading at malicious_x = 0xffffffffffdffb4f... Unclear: 0xAC=’?’ score=970 (second best: 0x54 score=968)  
Reading at malicious_x = 0xffffffffffdffb50... Unclear: 0xFC=’?’ score=973 (second best: 0x70 score=971)  
Reading at malicious_x = 0xffffffffffdffb51... Unclear: 0xBE=’?’ score=974 (second best: 0xBC score=972)  
Reading at malicious_x = 0xffffffffffdffb52... Unclear: 0xA3=’?’ score=977 (second best: 0xF1 score=974)  
Reading at malicious_x = 0xffffffffffdffb53... Unclear: 0x43=’C’ score=979 (second best: 0x1A score=973)  
Reading at malicious_x = 0xffffffffffdffb54... Unclear: 0x6C=’l’ score=980 (second best: 0x50 score=980)  
Reading at malicious_x = 0xffffffffffdffb55... Unclear: 0x72=’r’ score=980 (second best: 0x77 score=978)  
Reading at malicious_x = 0xffffffffffdffb56... Unclear: 0xBD=’?’ score=970 (second best: 0xF8 score=969)  
Reading at malicious_x = 0xffffffffffdffb57... Unclear: 0x61=’a’ score=983 (second best: 0x16 score=982)  
Reading at malicious_x = 0xffffffffffdffb58... Unclear: 0x65=’e’ score=976 (second best: 0x6D score=973)  
Reading at malicious_x = 0xffffffffffdffb59... Unclear: 0xC5=’?’ score=970 (second best: 0xB7 score=970)  
Reading at malicious_x = 0xffffffffffdffb5a... Unclear: 0xF6=’?’ score=982 (second best: 0xA8 score=982)  
Reading at malicious_x = 0xffffffffffdffb5b... Unclear: 0xAA=’?’ score=975 (second best: 0x3D score=975)  
Reading at malicious_x = 0xffffffffffdffb5c... Unclear: 0x1A=’?’ score=975 (second best: 0xB8 score=973)  
Reading at malicious_x = 0xffffffffffdffb5d... Unclear: 0xF4=’?’ score=985 (second best: 0x1C score=985)  
Reading at malicious_x = 0xffffffffffdffb5e... Unclear: 0xC0=’?’ score=978 (second best: 0xF6 score=977)  
Reading at malicious_x = 0xffffffffffdffb5f... Unclear: 0xA3=’?’ score=974 (second best: 0xB9 score=970)  
Reading at malicious_x = 0xffffffffffdffb60... Unclear: 0xB9=’?’ score=985 (second best: 0x1A score=984)  
Reading at malicious_x = 0xffffffffffdffb61... Unclear: 0x9F=’?’ score=979 (second best: 0x65 score=979)  
Reading at malicious_x = 0xffffffffffdffb62... Unclear: 0x1A=’?’ score=977 (second best: 0x73 score=971)  
Reading at malicious_x = 0xffffffffffdffb63... Unclear: 0x65=’e’ score=981 (second best: 0xB9 score=979)  
Reading at malicious_x = 0xffffffffffdffb64... Unclear: 0x73=’s’ score=978 (second best: 0x60 score=978)  
Reading at malicious_x = 0xffffffffffdffb65... Unclear: 0x65=’e’ score=974 (second best: 0xC5 score=972)  
Reading at malicious_x = 0xffffffffffdffb66... Unclear: 0xF2=’?’ score=967 (second best: 0x6F score=965)  
Reading at malicious_x = 0xffffffffffdffb67... Unclear: 0xA1=’?’ score=978 (second best: 0xAC score=977)
```

In this case, we got the string: `` =s??????oq??`r?????Clr?ae??????????ese?? ``

That’s significantly less information leaked.

I’m sure knowing that `-O2` was going to be applied we could then adapt the code to make it work. It did get a few characters right.

This demonstrates how messing with the branch predictor is not only possible, but that you can do it yourself and show your friends.

#### Digging a little deeper

So what just happened? Let’s look at the generated assembly in both cases to get an idea of whats going on:

Without any optimization flags, this is the default assembly for **victim\_function**:

```
root@47089c73b470:/# gcc -S spectre.c -o spectre.default.s  
root@47089c73b470:/# cat spectre.default.s   
<snipped/>  
victim_function:  
.LFB3695:  
 .cfi_startproc  
 pushq %rbp  
 .cfi_def_cfa_offset 16  
 .cfi_offset 6, -16  
 movq %rsp, %rbp  
 .cfi_def_cfa_register 6  
 movq %rdi, -8(%rbp)  
 movl array1_size(%rip), %eax  
 movl %eax, %eax  
 cmpq -8(%rbp), %rax  
 jbe .L3  
 movq -8(%rbp), %rax  
 addq $array1, %rax  
 movzbl (%rax), %eax  
 movzbl %al, %eax  
 sall $9, %eax  
 cltq  
 movzbl array2(%rax), %edx  
 movzbl temp(%rip), %eax  
 andl %edx, %eax  
 movb %al, temp(%rip)  
.L3:  
 nop  
 popq %rbp  
 .cfi_def_cfa 7, 8  
 ret  
 .cfi_endproc  
<snipped/>
```

When I added `-O2`, what did it do?

```
root@47089c73b470:/# gcc -S -O2 spectre.c -o spectre.o2.s  
root@47089c73b470:/# cat spectre.o2.s   
<snipped/>  
victim_function:  
.LFB4882:  
 .cfi_startproc  
 movl array1_size(%rip), %eax  
 cmpq %rdi, %rax  
 jbe .L1  
 movzbl array1(%rdi), %eax  
 sall $9, %eax  
 cltq  
 movzbl array2(%rax), %eax  
 andb %al, temp(%rip)  
.L1:  
 rep ret  
 .cfi_endproc  
<snipped/>
```

Let me lay out the “victim\_function” assembly code side by side so you’ll know what changed:

![](/assets/images/defeat-spectre-variant-1-diy-at-home/1_Qh53Y01S1pKTqxoWY80QSQ.png)

They still do the exact same thing. They still perform identically. They’re compiled using the same compiler. Both are legitimate. They are semantically identical, but they use different instructions in different orders, and look how easily the attack attempt was defeated.

We’ve been saying it for a couple of years now: the problem is a lack of diversity. The minute we change a few things here and there, the effort required of an attacker increases by orders of magnitude.

Now you can compile your own code with different optimizer flags and test the effects.

The Polymorphic Compiler that builds our [Polymorphic Linux](https://polyverse.com/how-it-works) does something quite similar, but with a higher cardinality and more effectiveness.

You can use Polymorphic Linux immediately through our [free tier](https://polyverse.com/polymorphic-linux-installation-guide/).
