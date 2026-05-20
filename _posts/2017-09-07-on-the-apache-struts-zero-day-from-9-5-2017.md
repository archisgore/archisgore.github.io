---
layout: post
title: "On the Apache Struts zero-day from 9/5/2017"
description: "A slashdot post caught my attention yesterday, enlightening me to this: https://lgtm.com/blog/apache_struts_CVE-2017-9805_announcement"
date: 2017-09-07 17:11:12 +0000
original_url: https://medium.com/@archisgore/on-the-apache-struts-zero-day-from-9-5-2017-f0f79c34e174
---

A slashdot post caught my attention yesterday, enlightening me to this: <https://lgtm.com/blog/apache_struts_CVE-2017-9805_announcement>

You should definitely read the first two paragraphs — especially the warning added on September 6th about multiple known exploits in the wild. I’m going to touch on that in a bit. Let’s dissect the issue and look at how we as a security industry handled it, reacted to it, and what our options were before it was announced, when it was announced, after it was patched. When I say options, I mean operational options that fix it in real life, not merely in a lab.

*When I say “we” — I mean the tech industry, programmers, security industry, management, auditors, compliance officers, etc. etc. Anyone who had the ability to do something about it. The same “we” when we ponder, “What can we do to increase the population of Pandas?”*

### The Background

#### The exploit, (intentionally) comically simplified

Let’s quickly cover this part. I’m massively simplifying here of course. How does this vulnerability look in real life?

Let’s say you have a web page that runs this particular version of Apache Struts with a REST plugin (any version before September 5th 2017 — that was over 48 hours ago as of the time of writing of this blog), then you could go to that website, and post some data on a URL, that could cause an arbitrary program on that web site’s server to be run. A comically simplified representation looks something like this:

```
HTTP POST "<xml>run 'cd / && rm -rf *'</xml>" → http://website-you-want-to-hack.com
```

Technically speaking, this isn’t all that different from the helpful ActiveX problems Internet Explorer had to deal with in the 90’s and early 2000s. It is a software that is very extensible, and being helpful in that you can tell it what to do, and it’ll do it. 99 out of a 100 times, that’s wonderful. That one time, a malicious person can tell it to do something… well, malicious, and *the software doesn’t know the difference*. Remember that quote — doesn’t know the difference. We’ll come back to it in a bit too.

#### This is a big deal, it doesn’t affect you, but affects “that friend you have”

If the blog post is to be believed, and they have a quote from the CISO of a Tier-1 bank, then a great deal of Fortune 100 companies are affected. So this is a very big deal in terms of surface area.

What makes this a bigger deal, and the big elephant in the room for everyone who’s in Ops/DevOps right now, is an innocuous quote at the end of this [Apache bulletin](https://struts.apache.org/docs/s2-052.html):

> *No workaround is possible, the best option is to remove the Struts REST plugin when not used or limit it to server normal pages and JSONs only:*

> `<constant name="struts.action.extension"``value="xhtml,,json"``/>`

Which is another way of saying — either turn off your critical services that depend on this, or hope you don’t get hacked while you remain unpatched.

#### Bad guys already have the exploit; Good guys pay for it

This is a personal pet peeve from someone who has gotten an email from his boss asking, “Hey, I need to know quickly — are we affected by this?”

Read that blog again — the first time I read it, I wanted to check if I was vulnerable. In the interest of safety, a working exploit was not published. There’s enough details in there for me to create my own exploit. Now look at the lopsided incentive structure here.

If I’m a good guy wanting to verify the existence of the bug in my system I have to work hard to reproduce it. Or I must risk visiting shady websites or the darkweb to get one. Simply to verify if I need to worry.

If I’m a bad guy wanting to exploit someone, my investment is easily justified, and the details are sufficient enough for me to generate it. Before they published that update on multiple known exploits in the wild, I was about to argue that they already exist, and I’m thankful the original authors mentioned that.

Going back again to the operational defender of an infrastructure, this habit, while it appears “cool” in fact costs the good guys and doesn’t really stop the bad guys. It doesn’t even stop the script kiddies because they aren’t going to be deterred from visiting shady websites. But someone sitting on a terminal inside a corporate VPN surely risks a LOT by having to visit them just to verify the exploit.

#### The real worst-case nobody talks about

Now we arrive at the absolutely frightening scenario that probably is already happening, and is being discussed behind closed-doors. What about attackers who have obtained persistence?

You see, getting your website defaced, having all your web servers DoS’d, or having data immediately stolen, are relatively GOOD things that can happen. It’s publicly humiliating. You have a PR nightmare on your hands. But in the big picture, it’s a dumb attacker.

The real attacker is instead launching agents across all machines they can get their hands on. They’re opening network ports, or starting ssh daemons, or creating user accounts for a huge payoff much later, after this storm has calmed. They’re establishing ***persistence***. If you didn’t get defaced, or made the news, thats when you REALLY begin to worry. The amount of outgoing traffic from within an organization can easily drown out persistent agents running somewhere that are phoning home for commands.

Persistence is a defender’s nightmare! We can have sleeper accounts, sleeper agents, sleeper ports, sleeper machines and sleeper processes in our networks for YEARS without knowing about it. Why steal measly data now, when you can short-sell right before that big multi-billion-dollar IPO or acquisition? That would be the cyber equivalent of the plot from [Casino Royale](http://www.imdb.com/title/tt0381061/).

### The Analysis

#### **Why didn’t we catch it? Can we catch it now?**

In a way, we did catch it — that’s why we’re fixing it. However, we didn’t catch it using any automation, rule sets, or detection methodologies. The easy excuse here is the Church-Turning thesis — there is no way we can write a computer program, to determine the behavior of another program. We simply can’t. It is theoretically impossible. This is a mathematical bound.

What should bother you a lot more though is that despite knowing of the flaw now, we still don’t know how to catch it, or mitigate it. Let me rephrase that — we know we cannot catch it.

We can catch similar expressions to my comically-simplified example above in our firewalls and perimeter defenses, and we might suffer from a massive false-positive rate, with no guarantee of completeness (as in ensuring that there will be NO false-negatives.) So this becomes an option where you pay a high cost and get not that much in return.

Static analysis tools — those things that look at software and tell you if it’s vulnerable or not — they also depend on signatures. They can at best look similar XML deserialization code and make some intelligent analysis of it. However, most of those tools simply look at a binary’s hash if they’re sophisticated, and a version number if they’re not — and they have a dictionary of all the things wrong with that binary, put there by a person. You’re limited by how much that person knew when they constructed that dictionary.

In short — we had nothing in our arsenal to catch/stop this, and in general this is a theoretically impossible problem to solve (I’m definitely not saying that there is a lot we can do right below “impossible” that still makes it difficult for an attacker.)

#### What can we do now, why haven’t we done it?

We have two options. The immediate option is what I like to call the BRS — Big Red Switch. Kill the services immediately. No negotiation. No thinking.

The option that takes a day or two, is that we patch. We patch like hell and we hope there’s someone else more important than us, that the attackers are going after. That’s all we can do.

You’ll notice that these two options are easier said than done. And this is also where traditional “Security” becomes a burden. Security’s job is done when they say, “Patch more! Patch faster!” The Ops/DevOps people are the ones who have to publish those patches, and get them rolled out. Patches need to be tested for reliability, and uptime, and functional validation and so much more. If patches break existing APIs or behavior assumptions, then there’s coding involved, and now developers have to get involved and pull all-nighters. This is not easy or pleasant, and it is a burden that predominantly InfoSec doesn’t really share in.

### The Fundamental Problems that keep this cycle going

I covered a lot of this when I wrote about what I think [DevSecOps needs to be](https://blog.polyverse.io/what-is-devsecops-5568304df5b7).

#### Institutional Stigma against assuming the hack

If I had spoken to ANY CISO, CSO, InfoSec, Security, SecOps person, and had mentioned the scenario where we assume Apache is already hacked, I would have faced a heavy backlash. I know because I did say it — last week, the week before, and the week before that.

You get the usual answers around:

1. We have excellent static detection. We scan all our binaries.
2. We have great firewalls and we can stop everything.
3. We have strict SELinux/AppArmor/Policies.
4. We don’t let Developers touch production.

Somehow, Assuming the Hack, is still misinterpreted as “Making a mistake”. This stigma really prevents further discussing or developing solutions for what it implies. Even today, unless a website gets defaced or someone releases some private database, you will still have trouble talking about assumption of a ***persistent threat already existing***. Because it is impossible! You have auditing agents. You are compliant. You have monitoring. There’s no persistent threats on YOUR boxes!

#### Insufficient Post-Exploitation Defenses

I touched on this on my DevSecOps blog post as well. Look at the asymmetry between Security Conference trainings, and security defense operations. Nearly half the trainings are BlackHat are what we call “Post-Exploitation” trainings. This means that a minor exploit is already assumed to exist somewhere, and the trainings talk about how to use that to gain further and deeper entry into the system. On the defense side, there is hardly any thought given to what happens “once someone is already in”.

Because of the institutional stigma against that assumption, and the implication of incompetence, there are very few tools we fund, prioritize or build to defend against someone who is already in. The thought is so frightening, that every time a vulnerability like this exists, we double down on stricter policies and stricter static analysis and stricter code reviews to ensure this kind of vulnerability never happens again!

Even Richard Pryor in Superman III built an inner defense for his computer assuming his external defenses failed. Surely, we can do better.

#### Insufficient Operational Viability of Security tools

Finally, and this is a big problem for Ops/DevOps, is that security recommendations/solutions/tools are rarely built with operational viability in mind. Some of them are almost impractical to implement. But even when practical to do, they must be operationally viable. What I mean by this is being built with the real life conditions in mind, not merely a lab setting. A lot goes wrong in real life, and when you build tools for the ops team, they must be massively scalable, repeatable, forgiving, self-correcting, and scriptable. Those beautiful UIs are a great sales tool. They are an absolute nightmare for an operator of a large fleet.

Let’s take patch management as an example. It doesn’t take a smart person to tell me that I should patch more. It does take an incredibly smart person to show me how they would patch much faster than I would, without sacrificing reliability or uptime and without working 80 hour weeks and without making mistakes.

### Closing thoughts

We in the security world, need to do better. We have to do better than “patch more!”. We have to take up the mandate of making things secure in real life, not lab environments — with all the dirt that comes with it. Priorities, deadlines, human exhaustion, platform limitations, breaking APIs, development costs, build times, QA times, deployment windows and much more.
