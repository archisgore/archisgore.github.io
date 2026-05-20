---
layout: post
title: "The SLA-Engineering Paradox"
description: "Why outcome-defined projects tend to drive more innovation than recipe-driven projects"
date: 2018-06-20 19:43:15 +0000
original_url: https://medium.com/@archisgore/the-sla-engineering-paradox-7de99ffb2692
---

In the beginning, there was no Software Engineering. But as teams got distributed across the world, they needed to communicate what they wanted from each other and how they wanted it.

Thus, Software Engineering was born and it was…. okay-ish. Everything ran over-budget, over-time, and ultimately proved unreliable. Unlike construction, from whence SE learned its craft, computers had nothing resembling bricks, which hadn’t changed since [pre-history](http://www.sjsu.edu/faculty/watkins/indus.htm).

But in a small corner of the industry a group of utilitarian product people were building things that had to work now: they knew what they wanted out of them, and they knew what they were willing to pay for them. They created an **outcome-driven approach**. *This approach was simple: they asked for what they wanted, and tested whether they got what they asked for.* These outcomes were called SLAs (Service Level Agreements) or SLOs (Service Level Objectives). I’m going to call them SLAs throughout this post.

The larger industry decided that it wanted to be perfect. It wanted to build the best things, not good-enough things. Since commoners couldn’t be trusted to pick and choose the best, it would architect the best, and leave commoners only to execute. It promoted a **recipe-driven approach**. *If the recipe was perfectly designed, perfectly expressed, and perfectly executed, the outcome would, by definition, be the best possible.*

The industry would have architects to create perfect recipes, and line cooks who would execute them. All the industry would need to build were tools to help those line cooks. This led to [Rational Rose](https://en.wikipedia.org/wiki/IBM_Rational_Rose_XDE), [UML](https://en.wikipedia.org/wiki/Unified_Modeling_Language), [SOAP](https://en.wikipedia.org/wiki/SOAP), [CORBA](https://en.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture), [XMLs DTDs](https://www.w3schools.com/xml/xml_dtd.asp), and to some extent SQL, but more painfully Transactional-SQL, stored procedures, and so on.

The paradox of software engineering over the past two decades is that, more often than not, people who specified what they wanted without regard to recipes tended to produce better results, and create more breakthroughs and game changers, than those who built the perfect recipes and accepted whatever resulted from them as perfect.

*(I can hear you smug functional-programming folks. Cut it out.)*

Today I want to explore the reasons behind this paradox. But first… let’s understand where the two camps came from intuitively.

### The intuitive process to get the very best

If you want to design/architect the most perfect system in the world, it makes sense to start with a “[greedy algorithm](https://en.wikipedia.org/wiki/Greedy_algorithm)”. At each step, a greedy algorithm chooses the absolute very best technology, methodology and implementation the world has to offer. You vet this thing through to the end. Once you are assured you could not have picked a better component, you use it. As any chef will tell you, a high-quality ingredient cooked with gentle seasoning will outshine a mediocre ingredient hidden by spices.

The second round of iteration is reducing component sets with better-together groups. The perfect burger might be more desirable with mediocre potato chips than with the world’s greatest cheesecake. In this iteration you give up the perfection of one component for the betterment of the whole.

In technology terms, this would mean that perhaps a SQL server and web framework from the same vendor go better together, even if the web server is missing a couple of features here and there.

These decisions, while seemingly cumbersome, would only need to be done once and then you were unbeatable. You could not be said to have missed anything, overlooked anything, or given up any opportunity to provide the very best.

What you now end up with, after following this recipe, is the absolute state-of-the-art solution possible. Nobody else can do better. To quote John Hammond, “[spared no expense.](https://www.youtube.com/watch?v=DqM0yZK__gI)”

### SLA-driven engineering should miss opportunities

The SLA-driven approach of product design defines components only by their attributes — what resources they may consume, what output they must provide, and what side-effects they may have.

The system is then designed based on an assumption of component behaviors. If the system stands up, the components are individually built to meet very-well-understood behaviors.

Simply looking at SLAs it can be determined whether a component is even possible to build. Like a home cook, you taste everything while it is being made, and course-correct. If the overall SLA is 0.2% salt and you’re at 0.1%, you add 0.1% salt in the next step.

The beauty of this is that when SLAs can’t be met, you get the same outcome as recipe-driven design. The teams find the best the world has to offer, and tells you what it can do — you can do no better. No harm done. In the best case however, once they meet your SLA, they have no incentive to go above and beyond. You get mediocre.

This is a utilitarian industrial system. It appears that there is no aesthetic sense, art, or even effort to do anything better. There is no drive to exceed.

SLA-driven engineering should be missing out on all the best and greatest things in the world.

### What we expected

The expectation here was obvious to all.

If you told someone to build you a website that works in 20ms, they would do the least possible job to make it happen.

If you gave them the perfect recipe instead, you might have gotten 5ms. Even if you did get 25ms or 30ms, they followed your recipe faithfully, you’ve just hit theoretical perfection.

A recipe is more normalized; it is compact. Once we capture that it will be a SQL server with password protection, security groups, roles, principals, user groups, onboarding and offboarding processes, everything else follows.

On the other hand, SLA-folks are probably paying more and getting less, and what’s worse, they don’t even know by how much. Worse they are also defining de-normalized SLAs. If they fail to mention something, they don’t get it. Aside from salt, can the chef add or remove arbitrary ingredients? Oh the horror!

### The Paradox

Paradoxically we observed the complete opposite.

SLA-driven development gave us Actor Models, NoSQL, BigTable, Map-Reduce, AJAX (what “web apps” were called only ten years ago), Mechanical Turk, concurrency, and most recently, Blockchain. Science and tech that was revolutionary, game-changing, elegant, simple, usable, and widely applicable.

Recipe-driven development on the other hand kept burdening us with UML, CORBA, SOAP, XML DTDs, a shadow of Alan Kay’s OOP, Spring, XAML, Symmetric Multiprocessing (SMP), parallelism, etc. More burden. More constraints. More APIs. More prisons.

It was only ten years ago that “an app in your browser” would have led to Java Applets, Flash or Silverlight or some such “native plugin.” Improving ECMAScript to be faster would not be a recipe-driven outcome.

The paradoxical pain is amplified when you consider that recipes didn’t give us provably correct systems, and failed abysmally at empirically correct systems that the SLA camp explicitly checked for. A further irony is that under SLA constraints, provable correctness is valued more — because it makes meeting SLAs easier. The laziness of SLA-driven folks not only seems to help, but is essential!

A lot like social dogma, when recipes don’t produce the best results, we have two instinctive reactions. The first is to dismiss the SLA-solution through some trivialization. The second is to double-down and convince ourselves we didn’t demand enough. We need to demand MORE recipes, more strictness, more perfection.

### Understanding the paradox

Let’s try to understand the paradox. It is obvious when we take the time to understand context and circumstances in a *social* setting where a human is a person with ambitions, emotions, creativity and drive, vs. an *ideal* setting where “a human” is an emotionless unambitious drone who fills in code in a UML box.

#### SLAs contextualize your problem

This is the obvious easy one. If you designed web search without SLAs, you would end up with a complex, distributed SQL server with all sorts of scaffolding to make it work under load. If you first looked at SLAs, you would reach a radically new idea : these are key-value pairs. Why do I need B+ trees and complex file systems?

Similarly, if you had to ask, “What is the best system to do parallel index generation?”, the state of the art at the time were [PVM/MPI](https://en.wikipedia.org/wiki/Parallel_Virtual_Machine). Having pluggable Map/Reduce operations was considered so cool, every resume in the mid-2000s had to mention these functions (something that is so common and trivial today, your web-developer javascript kiddie is using it to process streams.) The concept of running a million cheap machines was neither the best nor state of the art. :-) No recipe would have arrived at it.

#### SLAs contextualize what is NOT a problem

We all carry this implied baggage with us. Going by the examples above, today none of us consider using a million commodity machines to do something as being unusual. However, with no scientific reason whatsoever, it was considered bad and undesirable until Google came along and made it okay.

Again, having SLAs helped Google focus on what is NOT a problem. What is NOT the objective. What is NOT a goal. This is more important than you’d think. The ability to ignore unnecessary baggage is a powerful tool.

Do you need those factories? Do you need that to be a singleton? Does that have to be global? SLAs fight over-engineering.

#### SLAs encourage system solutions

What? That sounds crazy! It’s true nonetheless.

When programmers were told they could no longer have buffer overflows, they began working on [Rust](https://en.wikipedia.org/wiki/Rust_%28programming_language%29). When developers were told to solve the dependency problem, they flocked to [containers](https://www.cio.com/article/2924995/software/what-are-containers-and-why-do-you-need-them.html). When developers were told to make apps portable, they made [Javascript faster](https://en.wikipedia.org/wiki/Chrome_V8).

On the other hand, recipe-driven development, not being forced and held to an outcome, kept going the complex way. Its solution to buffer overflows was more static analysis, more scanning, more compiler warnings, more errors, more interns, more code reviews. Its solution to the dependency problem was more XML-based dependency manifests and all the baggage of semantic versioning. Its solution to portable apps was more virtual machines, yet one more standard, more plugins, etc.

### Without SLAs Polyverse wouldn’t exist

How much do I believe in SLA-driven development? Do I have proof that the approach of first having the goal and working backwards works? Polyverse is living proof.

When we saw the [Premera hack](https://www.healthitoutcomes.com/doc/premera-audit-revealed-vulnerabilities-prior-hack-0001), and if we wondered what the BEST in the world is, we’d have thought of even MORE scanning, and MORE detection and MORE AI and MORE neural networks, and maybe some MORE blockchain. Cram in as much as we can.

But when we asked ourselves “If the SLA is to not lose more than ten records a year or bust,” we reached a very different systemic conclusion.

When [Equifax happened](http://money.cnn.com/2018/02/09/pf/equifax-hack-senate-disclosure/index.html), if we’d asked ourselves what the state of the art was, we would do MORE code reviews, MORE patching, FASTER patching, etc.

Instead we asked, “What has to happen to make the attack impossible?” We came up with [Polyscripting](https://github.com/polyverse/polyscripted-php).

Welcome to the most counterintuitive, and yet widely empirically proven, and widely used conclusion in Software Engineering. Picking the best doesn’t give you the best. Picking a goal leads to better than anything the world has to offer today.

Google proved it. Facebook proved it. Apple proved it. Microsoft proved it. They all believe in it. Define goals. Focus on them. Ignore everything else. Change the world.
