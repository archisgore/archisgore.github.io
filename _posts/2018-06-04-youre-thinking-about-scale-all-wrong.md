---
layout: post
title: "You’re thinking about scale all wrong"
description: "Scale isn’t about large numbers"
date: 2018-06-04 23:58:06 +0000
original_url: https://medium.com/@archisgore/youre-thinking-about-scale-all-wrong-29b1ac764477
---

To hear modern architects, system designers, consultants and inexperienced (but forgivable) developers talk about scale, you’d think every product and service was built to be the next Twitter or Facebook.

Ironically, almost everything they create to be scalable would crash and burn if that actually happened. Even Google and Amazon aren’t an exception to this, at least from time to time. I know this because we run the largest build farm on the planet, and I’m exposed to dirty secrets about pretty much every cloud provider out there.

I want to talk about what scalability really means, why it matters and how to get there. Let’s briefly calibrate on how it’s used today.

#### Recap of pop-culture scalability

When most tech journalists and architects use the word scale, they use it as a noun. They imagine a very large static system that’s like… really *really* big in some way or another. Everyone throws out numbers like they’re talking about corn candy — hundreds or thousands of machines, millions of processes, billions of “hits” or transactions per second… you get the idea.

If you can quote a stupidly large number, you’re somehow considered important, impregnable even.

Netflix constitutes [37% of the US internet traffic](https://appleinsider.com/articles/16/01/20/netflix-boasts-37-share-of-internet-traffic-in-north-america-compared-with-3-for-apples-itunes) at peak hours. Microsoft famously runs “[a million](https://www.extremetech.com/extreme/161772-microsoft-now-has-one-million-servers-less-than-google-but-more-than-amazon-says-ballmer)” servers. Whatsapp moves a billion messages a day.

These numbers are impressive, no doubt. And it’s precisely because they’re impressive that we think of scale as a noun. “At a million servers,” “a billion transactions” or “20% of peak traffic” become defining characteristics of scale.

#### Why it’s all wrong

> Calling something “scalable” simply because it is very, very, very large is like calling something realtime only because it is really, really fast.

Did you know that nowhere in the definition of “real-time systems” does it say “really, really fast?” Real-time systems are meant to be time-deterministic, i.e., they perform some operation in a predictable amount of time.

Having a system go uncontrollably fast can quite frequently be undesirable. You ever played one of those old DOS games on a modern PC? You know how they run insanely fast and are almost unplayable? That’s an example of a non-realtime system. Just because it runs incredibly fast doesn’t make it useful. That it could act with desirable and predictable time characteristics is what would make it a realtime system.

What makes a system realtime is that it works in time that is “real” — a game character’s movements must move in time that is like the real world, the soundtrack of a video must play to match the reality of the video, a rocket’s guidance computer must act in a time that matches the real world. Occasionally a “real time” system might have to execute NO-OPs so that certain actuators are signaled at the “correct time.”

As with much of computing, the definition of scalability depends on the **correctness** of a system, rather than the size or speed of it.

#### Scale is a verb, not a noun

The biggest misconception about scale is that it is about being “at scale.” There’s no honor, glory, difficulty or challenge in that, trust me. You want to see a 10K node cluster handling 100M hits per second? Pay me the bill, you got it. I’ll even spin it up over a weekend.

The real challenge, if you’ve ever run any service/product for more than a few months, is the verb “to scale.” To scale from 10 nodes to 100 nodes. To scale from 100 transactions to 500 transactions. To scale from 5 shards to 8 shards.

A scalable system isn’t one that launches some fancy large number and just stupidly sits there. A scalable system is one that scales as a verb, not runs at some arbitrary large number as a noun.

#### What scalability really means

We commonly use the Big-O notation to define the correctness of behavior in an algorithm. If I were to sort *n* numbers, a quicksort would perform at worst ***n*-squared operations**, and it would take ***n* memory units**. A realtime sort would add the additional constraint that it would respond within ***n* minutes** on the wall-clock.

Similarly, a scalable system has a predictable Big-O operational complexity to adapt to a certain scale.

Meaning, if you had to build a system to handle *n* transactions per second, how much complexity do you predict it would take to set it up?

**O(*n*)? O(*n*-squared)? O(e^*n*)?**

Not really an easy answer is it? Sure we try our best, and we question everything, and we often really worry about our choices at scale.

But are we scale-predictable? Are we scale-deterministic? Can we say that “for 10 million transactions a second, it would take the order of 10 million dollars, and **NO MORE,** because we are built to scale”?

I run into a dozen or so people who talk about large numbers and huge workloads. But very few people who can grow with my workload, with incremental operational costs.

Scalability doesn’t mean a LOT of servers. Anyone can rent a lot of servers and make them work. Scalability doesn’t mean a lot of transactions. Plenty of things will fetch you a lot of transactions.

Scalability is the Big-O measure of cost for getting to that number, and moreover, the predictability of that cost. The cost can be high, but it needs to be known and predictable.

#### Some popular things that “don’t scale”

Hopefully this explains why we say some things “don’t scale.” Let’s take the easiest punching bag — any SQL server. I can run a SQL server easy. One that handles a trillion transactions? Quite easy. With 20 shards? That’s easy too. With 4 hot-standby failovers? Not difficult. Geographically diverse failovers? Piece of cake.

However, the cost of going from the one SQL instance I run up to those things? The complexity cost is this jagged step function.

![](/assets/images/youre-thinking-about-scale-all-wrong/1_p-fxgKLQn-yJJuTiw1mkoA.png)

A lot of unpredictable jagged edges

And I’m only looking at a single dimension. Will the client need to be changed? I don’t know. Will that connection string need special attention? Perhaps.

You see, the difficulty/complexity isn’t in actually launching any of those scenarios. The challenge is in having a predictable cost of going from one scenario to a different scenario.

#### Why should this matter?

> I’m advocating for **predictable growth** in complexity.

Let’s talk about my favorite example — rule-based security systems. Does any rule-based system (IPTables, firewalls, SELinux, AuthZ services) handle 10 million rules? You bet. If you have a static defined system that is architected on blueprints with every rule carefully predefined, it’s possible to create the rules and use them.

Can you smoothly go from 10 rules to 10,000 rules on a smooth slope? Paying complexity as you need it?

![](/assets/images/youre-thinking-about-scale-all-wrong/1_vxavy2eXPj6M9nQ6eBdArg.png)

This is hardly ever the case. You might think that I’m advocating for a linear growth in complexity. I’m not. I’m advocating for a **predictable growth** in complexity. I’d be fine with an exponential curve, if I *knew* it was exponential.

What makes it unscalable, isn’t that the cost is VERY high, or that it is a predictable step function. What makes it truly unscalable is that the complexity is both abruptly and, worse, unpredictably step-py. You will add 10 rules sometimes. Add an 11th rule and it causes a conflict that leads to a 2-day investigation and debugging! You might add 100 nodes with ease. Add an extra node past some IP-range and you’ll be spending weeks with a network-tracer looking for the problem.

An example a bit closer to home. We’ve been looking for a home for Polyverse’s BigBang system — the world’s largest build farm that powers all the scrambling you get transparently and easily.

> As an aside, you’ll notice that Polymorphic Linux is “scalable.” What cost/complexity does it take for n nodes? Whether that n be 1, 100, 10,000, 10,000,000? The answer is easily O(n). It is sub-linear in practice, but even in the worst case it is linear. There are no emergency consultants, system designers or architects required to rethink or redesign anything. This is an example of what good scalability looks like.

Behind the scenes of that scalability though, is another story. I’ve spoken to nearly every cloud provider on the planet. I may have missed a few here and there, but I bet if you named a vendor, I’ve spoken to them. They all have “scalable systems,” but what they really have are various systems built to different sizes.

![](/assets/images/youre-thinking-about-scale-all-wrong/0_Ai9qe-Ni9SusE2cE.jpg)

Finding clouds/systems/clusters that can just run really, *really* large loads is easy. Running those loads is also easy. Finding clouds that are predictable in complexity based on a particular load? Even with all the cloud propaganda, that’s a tough one.

#### Cybersecurity needs more scalable systems, not systems “at scale”

> Scalable systems are not about size, numbers or capability. They have a predictable cost in the dimension of size.

Hopefully I’ve explained what scalable really means. In much the same way that you’d measure a system in number of operations, amount of memory, number of transactions, or expected wall-clock time, a scalable system is operationally predictable in terms of size.

It doesn’t have to be cheap or linear. Merely predictable.

Cybersecurity today is desperately in need of solutions that “can scale,” not ones that merely run “at scale.” We need scalable solutions that encourage MORE security by adding MORE money. Not haphazard, arbitrary and surprising step functions.
