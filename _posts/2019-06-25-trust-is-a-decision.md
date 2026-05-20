---
layout: post
title: "Trust is a Decision"
description: "A guide on “How to Trust”"
date: 2019-06-25 15:21:49 +0000
original_url: https://medium.com/@archisgore/trust-is-a-decision-59b7591b46d4
---

On the eve of [Re:Inforce](https://reinforce.awsevents.com), I wanted to share my thoughts on how to trust, [elaborating upon something I mentioned in a recent podcast](https://www.uberknowledge.com/archis-gore-cto-polyverse/). This post is as applicable socially as it is technologically.

All of us struggle with trust to some extent. It is easy to know what you don’t trust. A person with serial criminal charges. A company with consistently bad behavior. It is far harder to proactively trust… because this person, this entity has not done anything bad — or at least that I know of? Doesn’t have quite enough strength to it.

#### Intuitive attempts to improve trust don’t always work

Have you put your trust in people and they let you down? Did you sign all your binaries, yet someone still managed to inject a backdoor, or leaked private keys? Did you give your developers freedom, only to feel embarrassed on launch day?

We tend to have a number of natural reactions after an untrustworthy event:

1. **Increase deterrence**: More code reviews. More code coverage. Double the signatures on every binary. More static analysis. More compliance rules. Withholding rewards. Suspicion. Paranoia. If they won’t listen to me the easy way, I’ll make them do it the hard way. *Unfortunately, this only punishes everyone who didn’t mess up, but in no way deters those who did.*
2. **“Do” something**: This is the equivalent of searching for your keys on the street where the light is, rather than the pitch-black garage where they were lost. Can we use some OOP design pattern? Blockchain might help — it has that “crypto” in it. Maybe using the lessons Netflix learned playing videos around the world might help. Chaos Monkey? Simian Army? That new scanner tool uses AI now. And so on. This is a perfectly normal reaction.*When we feel helpless, it can be very comforting just to have some sort of agency.*
3. **Make it somebody else’s problem**: Maybe the problem was that one programmer. What if we open-sourced it? If more eyeballs see it, it will be better. How about publishing our binary on a blockchain? Then the group can build “consensus” about it. Consensus will make it a better binary. The problem is, as George Bernard Shaw famously said, “*Do not do unto others as you may have them do unto you. They may not share the same tastes.*”

In a way, our minds are attempting to reconcile the sunk-cost fallacy (having lost $100 on a project with no results, maybe if I invest only $10 more, I’ll get a $200 payoff) with the law of averages (if ten of my past projects failed due to chance, the next is bound to succeed; I can only fail so many times.)

#### Trust is not an attribute or property; nor is it a lack thereof

The biggest fallacy with trust is to mistake it for an attribute or property.

*The software went through 20 code reviews, has 80% code coverage, and was signed three times. Seems fairly trustworthy to me!*

*My bootloader is overly complex and does 800 steps, documented in an 8,000-page architecture diagram. Must be trustworthy.*

*This message came through a Blockchain. Five thousand people copied it and verified its hash. How can it be false, if 5,000 people verified the hash?*

Sarcasm aside, there is something good to attributes though. There’s some value to having code reviews, code coverage and signatures. A trustworthy bootloader is definitely going to be a bit more than starting at address 0x0000 and executing what it finds. There’s definitely value in 5,000 people coming together and cooperating.

So what is the line between progress and parody? It is mistaking that trust arises *after* you’ve done all the right things.

#### Trust is not an outcome

> It is not so much that entities are untrustworthy, but that we misplace our trust.

Trust does not arise naturally or organically because certain things happened.

You wouldn’t go through life buying every car before you settled on one you liked. You may not even want to *try* every single car without some idea of the features that you want. Nor would you eat cardboard because you don’t personally know of anyone having been killed by ingesting it. You definitely wouldn’t ingest poison oak because 5,000 people agreed that it is indeed, poison oak.

This is all just information. Having more of it, without direction, doesn’t materially change anything. Five thousand people agreeing on whether something is poison oak has value, if you had previously *decided whether or not poison oak was safe to eat*. A car is easy to settle on, if you knew that there exists no better car for what you had previouslydecided *you wanted out of it.*

The way we intuitively reason and are taught about trust is in the wrong direction. We are taught to trust that individual because they are a Buddhist monk. We are never taught to figure out what outcomes we desire, and ask what about a Buddhist monk makes them trustworthy.

The used-car-salesman stereotype will always go along the lines of, “*You can trust me because I sign all my binaries and statically analyze them.*”

The irony is: I do trust them. It is not so much that entities are untrustworthy, but that we misplace our trust. I would trust a cat to hunt mice. If I had a mouse farm, I would trust a cat to continue to hunt my mice.

A tip on how to effectively parse information: Rephrase anyone’s trustworthiness pitch to start with with your desired outcome, “*Since you, my customer, want the outcome of non-backdoored machines, I promise to insure you for $1 million, against your total data risk of $500K. I accomplish this outcome by signing binaries, which is where the highest risk of backdooring comes from.*”

#### Trust is a decision

> Trust is the decision you make to demand outcomes.

The decision of what side-effects you want/can tolerate has to come first. The assurance that you/your employees/vendors/contractors are doing no less than necessary, and no more than sufficient comes right after.

**First** comes the decision to seek certain outcomes. **Second** come the verifications that are **necessary and sufficient** to ensure the outcomes.

**Trust is the decision you make to demand outcomes.**

#### Making the decision sets everyone (except malicious actors) free

> Trust as a decision scares the crap out of malicious and sleazy actors, while empowering those who defend your interests.

Remember how I wrote about [Security as a function of asymmetry](https://blog.polyverse.io/how-to-think-about-security-asymmetry-of-difficulty-498eeebe91b5)? Everything worth having in life is about asymmetry of incentives.

Making the decision to trust works in much the same way — unfairly rewarding the good and unfairly punishing the bad.

Here’s a common scenario I hear from my banking and AI friends. My banking friends would love nothing more than to have certain complex analytics problems solved. My AI friends would love nothing more than to solve those problems. The challenge? Data regulations such as Europe’s General Data Protection Regulation (GDPR), which is seen as a burden. (Personal note: I’m extremely pro-GDPR; I don’t think it’s an unnecessary burden. Open to changing my mind.)

As a consumer how do you feel about your bank or physician sharing data with a third-party company? I feel very uncomfortable. What can they do to make us feel good? Prove that they are HIPAA compliant? Prove that they have security controls? Prove that they are deadly serious? I suppose these are good steps in the positive direction, but what, for example, do I know about HIPAA compliance and its applicability?

Let’s twist the argument around and look for side-effects we want instead. If I deposit a dollar in my bank, then at some arbitrary day in the future, any day of my choosing, I MUST be able to withdraw that dollar. That is what I want. That is the outcome I desire.

I couldn’t give two entries in a blockchain on how the bank ensures this happens. I don’t want to know. I don’t care. What I do care about is what the bank has to lose if they do not give me at least a dollar back. Let’s say the bank has to pay me $10 every time they screw up my dollar. I’d be pretty satisfied with that incentive structure. I would diversity my $100 across 10 banks, knowing that they are all competing NOT to lose $100 on my $10.

See how this has two immediate effects. Some banks that may be complaining about regulation, will start desperately wishing that regulation was all that was required of them, rather than direct, personal, material loss. The banks that ARE trustworthy, however, will gladly sign said contracts and win business. If they want to share my data with an AI company, they’re going to make damn sure that data is protected and, frankly, at that point I don’t even care if my data gets lost or stolen. Because I’m assured of getting either $10 or $100 back.

**Trust as a decision scares the crap out of malicious and sleazy actors, while empowering those who defend your interests.**

So here’s my hopeful guidance for everyone jaded with trust issues — decide when to trust. Ask what side-effects of the existence of a vendor, technology, methodology or action are. **Decide to trust, don’t get bullied into it.**
