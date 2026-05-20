---
layout: post
title: "Do not fret AI, computing professionals"
description: "Begun the programming language war has… again!"
date: 2026-01-15 05:25:03 +0000
original_url: https://medium.com/@archisgore/do-not-fret-ai-computing-professionals-7e85e9ba968b
---

If you were around in the 70s or 80s, you probably witnessed a cambrian explosion of programming languages — Fortran, Lisp, Prolog, Smalltalk, structured programming, OOP, Lambda Calculus, and more.

Each of these solved a credible need, which is why modern languages are so often multi-paradigm.

AI is going to eliminate “coding”, or so LinkedIn tells me when I bother to login. I’ll bet at least half the people even believe it.

I’m here to assure you, not only are your jobs safe, but they’re about to get a hell of a lot worse (translation: massive job security).

We’ve heard a lot of takes on why programming isn’t really coding, or the artistry of it and what not. It’s all true, I guess, but there’s a far more pragmatic reason we’re missing.

### AI is not Super-Turing

(Super-Turing or Hypercomputation: <https://en.wikipedia.org/wiki/Hypercomputation>)

That pesky Church-Turing thesis comes to haunt us, because no AI can do “more computer” than the computer that flew on the Apollo missions. Actually it really can’t do “more computer” than [Charles Babbage’s Analytical Engine](https://en.wikipedia.org/wiki/Analytical_engine) from the year when Victoria became Queen, which we [now know to be Turing-Complete](https://cstheory.stackexchange.com/questions/14262/was-babbages-analytical-engine-really-turing-complete), thanks to [Ada Lovelace having invented loops](https://softwareengineering.stackexchange.com/a/149484) 80 years before Alan Turing.

This isn’t to dunk on the amazing utility it provides, but to point out all the problems it cannot solve, because a Turing Machine can’t solve them!

Basically — AI cannot verify other AI because at best, every AI is only Turing Complete. And that means we cannot look at it, it’s prompt, generated code, and even determine that it will even halt, much less determine that it won’t do anything worse.

### The Prediction: We’re going to reinvent Lisp, but in AI!

I’m going to build up my argument, but if you already buy this premise, then here’s the punchline:

In the next year we will see:

1. Sklisp —[Greenspun’s 10th rule](https://en.wikipedia.org/wiki/Greenspun%27s_tenth_rule) will apply — there will be a Great Debate around the granularity of Claude Skills. Should we start with a skill for elementary counting principles, and build number theory on top, to solving prime factorization?
2. Chat++ — Holy War over whether Agents may inherit from other skills, or call them. Should the agent for “Book Airline Tickets” and “Book Taylor Swift ticket” call into a “Book Tickets” agent, or inherit from it and append to it’s prompt?
3. Claurlang — Skills are really agents that we run in a global context, and context pollution is a functional and security risk. We should created thousands of small contexts per skill that provided clean inputs and read outputs. Each skill should be a complete agent spun up on demand and killed when done.
4. LVM — Inevitably, LLM builders are going to build moats. And we’ll have a Virtual LLM that abstracts over all the moats.

### Why I think this

I spent a decade in cybersecurity. While the most popular solutions are antiviruses and what not, the real problem that we all face, is that at best we want to Figuring out Intent, and at worst, we want to [know the Impact.](https://archisgore.wordpress.com/tag/threat-intelligence/)

#### What is [Context Pollution](https://medium.com/@d98malik/agentic-ais-silent-killer-context-pollution-577562e09926), if not Shared Mutable Globals?

What is the difference between these two?

```
int p = 5  
int q = 5  
adjustValue(p)  
p = p + q  
modifyValue(q)  
q = p + 3  
modifyValue(p)  
p = p - 2  
// Why is p 52 when it should clearly be 25?
```

vs

```
"When I say machine the first time, it should be my web machine.   
But if the agent 'do-database-cleanup' was called, then use   
the first machine referenced before that agent was called. Keep track of all  
machines referenced throughout."
```

#### What is malware signature detection, if not prompt detection?

Instead of hashes of binaries, we’ll be sending hashes of text strings. A popup warning you that “The agent you’re about to call has, in it’s prompt, an instruction to delete all files” doesn’t seem far fetched.

Armies of people will be developing malicious prompts and larger armies will develop dictionaries for them.

It’s going to take one Agent or Skill to pollute someone’s context before a cottage industry of cybersecurity industries pops up to protect you.

#### What is Privacy if not Heap Isolation?

Say you have the greatest agent in the world, but you don’t want having it the full context of what you’re doing — both to avoid pollution and maybe because you don’t trust it/the authors. So now we’re passing developing public, private and protected members?

Which is to say — hang in there. There will be may jobs eliminated until the first black swan event, and then there’ll be even more demand for vetting prompts, isolating AIs, isolating contexts — we’re about to rediscover isolated heaps, protected class members, not using globals everywhere, and all of the past 50 years of programming learning all over again.

We’re going to need ADB (AI Debugger) with breakpoints at various steps. We’re going to need stack traces when an agent fails. Strap in because the next 20 years of work is about to be created, and everything you know is going to be useful.
