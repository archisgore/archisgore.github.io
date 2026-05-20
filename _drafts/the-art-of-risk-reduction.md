---
layout: post
title: "The Art of Risk-Reduction"
description: "The secret is to make stupid sentences impossible"
date: 2026-05-20 17:47:51 +0000
---

At first glance, I can be mistaken for a cavalier person. Both socially and professionally. I famously don’t care for code reviews, and my apathy towards code coverage and branch coverage testing is legendary. I work for a cybersecurity company, and I yet I frequently tell people I don’t care much for code-signing either. Oh and I wrote about why threat models suck. That’s pretty much all the things that should keep me safe.

The truth is, I care about risk. Empirically this shows (things don’t continually tend to fall apart), but I wouldn’t shy away from scientific rigor if you asked.

So how does a 20 person startup build 8 complete distributions of linux, 200 times a day and serve them to a few thousand machines around the world seamlessly? Yet keep the whole thing secure, reliable and resilient?

The secret is to make **stupid statements impossible**. I already wrote about how [my coffee may kill me](https://blog.polyverse.io/threat-models-suck-100ab95db8f5). What I never covered is how to know it’s not a problem.

### How a potato farmer may threaten the nuclear deterrence system

Let’s design a system for deterring anyone attacking this country, by designing a fail-safe guaranteed retaliatory strike. We’ll do the usual stuff — background checks for everyone involved, continual moving of the nukes, decentralized communication systems, and a supply chain for the operators….. well….

When someone the chain of whataboutism and whatifism, they arrived at the risk vector of potatoes. If the enemy turned the potato farmer, then they may grow tainted potatoes that may cause the nuke operator to have a bad day, leading to a delay in response when needed!

The solution though is obvious. Institute background checks on the potato farmer, regulation of the farming techniques used, complete monitoring of the supply chain. But wait a minute, the whataboutism experts say, what about the fertilizer that the potato farmer is buying? Fortunately, they can design you a system to supervise that too! Aren’t they the best?

That’s a fairly stupid statement in my book. Not because it is impossible, but because it is very much possible. If I was the consumer, I’d be stressed out, paranoid, frightened, and anxious all the time! Does that describe you or someone you know? Fear not my friends, I bring hope….

### **Make stupid statements impossible**

We have to understand why the statement about the fertilizer-supplier putting the nuclear deterrence system at risk, is a **stupid statement**.

The intuitive easy answer is that it sounds too convoluted for an attacker to try and pull it off. That doesn’t quite work, does it? You still feel anxious after saying it. If you spoke to a systems analyst or a trained operative, you’d feel even more anxious. If the movies and TV shows didn’t convince you, history should have. This is entirely possible, within the means of many people, and very likely. The reason it’s a stupid statement is because **we gave the fertilizer provider the agency to affect us**.

The only way to overcome anxiety is to KNOW that this is impossible. Not supervised or regulated. Not guarded. Not legally prosecutable. But impossible.

Turns out there is a way to verify potatoes won’t poison me. I can use the potato-analogues of canaries! I’m not concerned with how the potatoes got made, the political allegiance of the farmer, the nationality of the fertilizer-maker. That’s all a sham. That’s bullshit. What I am concerned about is the potatoes I’m about to eat right now!

First, let’s define the bounds of what I want to guard against. I definitely don’t want death. I definitely don’t want poisoning or indigestion. But can I tolerate just bad texture, or taste? Sure.

In that case, are there ways I can test this particular potato for non-fatality? Quite possibly so. Now, I only need to regulate testing, I control it, I own it, and I can scale it out to all the other food sources I have. When the tests fail, I can arrest the farmer and investigate whether it was mere incompetence or a conspiracy. The honest farmer is benefited of no burden. The malicious farmer is threatened with retaliation. I continue to eat potatoes, because I’ve verified that “*these potatoes are good*”, not that “*the potato supply chain is enemy-free.*”

### **We just tested in production!**

Let’s come back to software systems (though this applies to many other parts in life.)

The design of a good system rewards those who do the right thing, and retaliates against those who do malicious things. The design of a great system requires agency to act malicious, i.e. if you just bought potato seeds, planted them, grew them then you’ll never know the system exists. If you went out of your way to buy toxic fertilizer, you’ll know that it exists and you’ll have some stern words.

I’ve written before how we use secure channels to communicate artifacts using their hashes. Instead of exchanging signing keys, protecting the signing authority, having an elaborate system to sign on secure boxes, etc. we just exchange hashes with each other. What is the purpose of signing anyway? To inform a consumer that the artifact sent to them is the exact one the signer wanted them to have. There’s one accurate, infallible and reliable way to convey that — it’s hash!

When a program runs, it may have tolerable side-effects — memory consumption, CPU consumption, network calls, etc. You can either review code and run a turning machine in your head, or you can simply reject anyone who breaks those side-effect guarantees.

I am a huge fan of Tests-which-enforce-Spec. I actually discourage writing Specifications in Wikis and Documents. A specification is a behavioral contract. Write code to verify behavior you want.
