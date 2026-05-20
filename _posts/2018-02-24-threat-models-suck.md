---
layout: post
title: "Threat Models Suck"
description: "They’re everything that’s wrong with cybersecurity"
date: 2018-02-24 19:04:05 +0000
original_url: https://medium.com/@archisgore/threat-models-suck-100ab95db8f5
---

The coffee I’m sipping right now could kill me. You think I jest; but I assure you, if you work backwards from “death”, there is a possible precondition for some very deadly coffee. I just brewed another pot. I survived it to the end of this post. I love living on the edge and ignoring threats.

In cybersecurity though, we love our threat models. We think they’re smart and clever. Intuitively they make sense; in much the same way that a dictatorship and police state make sense, or nearly all the dystopian science fiction AIs make sense. If we programmed the AI to “keep us safe”, it is going to reach the optimal annealed solution: Remain under curfew, work out, stay isolated, don’t interact, and eat healthy synthetic nutritional supplements.

I’ve hated Threat Models since the day I had the displeasure to build one a decade ago. The first, and easy problem with them, is that they are product/solution driven; they’re rhetorical. Any credible threat model should have 80% of threat mitigations as “shrug.” When we don’t have a way to react to a threat, we subconsciously consider it non-existent. Nearly all threat models are playing [jeopardy](https://www.jeopardy.com/) (pun intended).

The second and more subtle problem is they encourage social grandstanding. How do you become a “more serious cybersecurity expert”? By coming up with a crazier threat vector than the last person.

“What if that CIA agent, is in reality, an NSA operative who was placed there by MI6, in order to leak the NOC list to MI6? Have you ever considered that? Now stop isolating that [XML deserializer](https://issues.apache.org/jira/browse/COLLECTIONS-580) like some kind of [pure functional programming evangelist](https://softwareengineering.stackexchange.com/questions/15269/why-are-side-effects-considered-evil-in-functional-programming), and let’s do some cybersecurity! Booyah!”

This is why we keep coming up with crazier and crazier tools when overlooking the obvious. I still cringe when someone calls Meltdown and Spectre “timing attacks”. The problem isn’t that the cache is functioning as it is and that you can measure access times. The problem is in **shared state.** But that doesn’t sound sexy and you can’t sell a 50 year old proven concept. Linus has perhaps the most profound quote in Cybersecurity history: [Security problems are primarily just bugs](https://it.slashdot.org/story/17/11/20/1412241/security-problems-are-primarily-just-bugs-linus-torvalds-says).

Adding jitter to timers, however, is clever, sexy, complicated, protects jobs, creates new jobs, and gets people promoted. Removing shared state across threads/processes is just a design burden that mitigates any impact (and solves a bunch of other operational problems while at it.)

### Impact Model

I propose we build *Impact Models*. Impact Models help prioritize investments, they help us make common sense decisions, but more so, they help us course-correct our decisions, by measuring whether the impact is mitigated/reduced.

In one of my talks aimed at Startups to prioritize security investments correctly, I use this slide.

![](/assets/images/threat-models-suck/1_C4a0HHcw_QkhJnrJWLc4Sg.png)

Why are you investing in Cybersecurity anyway? Is it to run cool technology? Is it to do a lot of “Blockchain”? Or is it to reduce/mitigate impact to business?

Just because something is technically a threat, doesn’t imply it has an appreciable impact. You’ll notice in the slide above, if I were to lose an encrypted laptop, it’d be incredibly inconvenient, painful and frustrating. However, Polyverse as an entity would suffer very little. How or Why I might lose the said laptop becomes less of a concern, since I’ve mitigated the impact of losing it.

This applies to our website too. We try to follow best practices. But beyond a certain point, we don’t consider what happens if AWS were to be hacked, and these legendary AWS-hackers were interested in defacing the website of a scrappy little startup above all else. Would it annoy me? You bet. Would it be inconvenient? Sure. Would it get a snarky little headline in the tech press? Absolutely. But would it leak a 150 million people’s PII? Not really.

Another benefit of impact modeling, is that it can present potentially non-“cybersecurity” solutions. I usually present this slide which is similar to Linus’s quote.

![](/assets/images/threat-models-suck/1_lIh-go7dJ33vj5TbA2L6xA.png)

Focussing on preventing a threat is important, and you should do it for good hygiene. Reducing the impact of that threat breaking through anyway, gives you a deeper sense of comfort.

We don’t live our lives based on threat models. We live them based on impact models. You’ll find that they will bring a great deal of clarity in your cybersecurity decision making, they’ll help you prioritize what comes first and what comes next. They’ll equip you to ask the right questions when purchasing and implementing technology. Most of all — they’ll help you get genuine buy-in from your team. Providing concrete data and justification motivates people far more than mandates and compliance.

#### “My threats are already sorted by impact!”, you say

I knew this would come up. Indeed every threat model does have three columns: **Threat Vector**, **Impact**, **Mitigation**.

Without impact you wouldn’t be able to pitch the threat seriously. InfoSec teams are nothing, if not good at visualizing world-ending scenarios. Much like my coffee’s purported impact of “death” got you this far, as opposed to “mild dehydration”.

![](/assets/images/threat-models-suck/1_Aq-UgK8MDlbx3K-PB9vsAA.png)

Threat Model

The problem is, read that Mitigation column and ask yourself what it’s mitigating. Is it mitigating the **Threat Vector**, or is it mitigating the **Impact**?

This is not a syntactical difference, it’s a semantic one. Multiple threats can have the same impact. Mitigating the impact, can remove all of them — even if some new threat is announced, which would have led to the same impact, you remain unconcerned. Your reaction is, “no change.”

![](/assets/images/threat-models-suck/1_UmKvW53BTHNA8tG7dp_jpQ.png)

Impact Model

In short, if Equifax had changed the “**if**” to “**when**”, they’d have had a much smaller problem to deal with.

Wishing you all a reduced impact.
