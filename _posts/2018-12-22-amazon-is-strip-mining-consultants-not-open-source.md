---
layout: post
title: "Amazon is strip-mining Consultants, not Open Source"
description: "Software Development is a great business model; Software-installation never was"
date: 2018-12-22 22:38:52 +0000
original_url: https://medium.com/@archisgore/amazon-is-strip-mining-consultants-not-open-source-9ef92ecaab71
---

If you’ve been following the latest incarnation of OSS licensing drama, you’ll see hints of the [BLA (Bad Linux Advocacy)](http://www.softpanorama.org/OSS/bad_linux_advocacy_faq.shtml) fervor of the late-90’s/early-2000’s, the OpenStack drama of the early 2010’s, and the Kubernetes drama being played out today.

Like my therapist says about all my terrible relationships — if everyone turns out to be toxic in the same manner, ask what was common across all of them?

Microsoft and Windows may have lost. Linux may have won. If you weren’t around 15 years ago, Linux didn’t create the jobs everyone thought it would. Supposedly, if Linux was capable, but needed babysitting, there’d be a cottage industry of Apache-configurators, NIS/LDAP maintainers, NFS-managers, etc. If I had to name the category, it’d be the DBA. Those jobs never materialized, and Red Hat became the scapegoat for making it too simple.

On the other hand, open source jobs and businesses are thriving. Building software is making all the money in the world. Employers are complaining about being ghosted — candidates who sign offers but never show up, candidates who tell you they quit by disappearing, and so on. More people are being paid more money to develop software in the open. Your Github account is your resume.

So what exactly is the deal with community licenses, Amazon hosting open source “without giving back”, etc. Are we really sore that Amazon isn’t giving back, or are we sore that they figured out the trap? I’ll come to this in a bit when we discuss OpenStack, the Amazon-killer from 2014.

#### The Consultant Trap

> Money is the cheapest resource you have.

Do you remember OpenStack? It was absolutely horrible — I’ve run it.

OpenStack had to accomplish two mutually exclusive tasks at the same time.

If you were a >$1billion dollar revenue company with 100 dedicated people, it had be easy for you to run. If you were less than that, it had to be difficult.

If it were trivial, it would destroy Amazon, but nobody would pay YOU. If it were too difficult but desirable, Amazon had all the capital to do it better than you.

It had to allow IBM, Rackspace, VMWare, etc. to take customers away from Amazon, while all at once, preventing a thousand others from doing the same.

OpenStack upstream actively fought attempts to make it simple, usable or obvious, because that was going to be why you’d want a vendor. If you dare me, I’ll hunt down pull requests, bug reports and issues to prove the point.

It’s a trap, because it is a consciously maintained gap (like a trapdoor function in cryptography), where the key looks like you. If you fill in that gap, the thing works. If you are not present, it doesn’t. That’s how you distribute a free product, but ensure you will be paid.

#### **Why Amazon?**

Can you imagine Microsoft or Google just simplifying something because you want it? Something that didn’t come from their billion-dollar research divisions based on running things at “mission-critical enterprise” for the former, or “galaxy scale” for the latter? They will pontificate around the Parthenon in Togas in order to figure out the true meaning of what “a database” is, and what higher-order manner the world should see the data.

It’d also become a “promotion vehicle” to tack on pet-projects (that’s what really killed the Zune, Windows Phone, Windows 8 and so on.) It’d have to have vision, strategy, synergy, dynamism, a new programming paradigm for someone’s promotion, a new configuration language for another promotion, etc.

Amazon scares the crap out of me. They’ll spin up a team of ten people, give them all the money and ask them to solve the damn problem in the simplest, stupidest way possible; promotions, titles, orgs be damned. Bezos has heard that a customer wants a cache. Make the cache happen or go to hell. You can see them doing that comfortably.

#### So how do you beat Amazon?

> Make it EASY!

Have you learned nothing? I have two examples: WordPress, and RedHat. Amazon would never host WordPress. What can they do except make it painful, complicated, behind that terrible UX of theirs? What it doesn’t do, is require a painful number of consultants for getting started. Despite being easy, the WordPress hosting business is thriving!

Red Hat is similar. It works. Oracle copied the code base and slashed the price in half; got nowhere. If you really worry about complex kernel patches, you’re going to pay for Red Hat’s in-house expertise. If you don’t need them, half-the-price doesn’t do much.

Every project should learn from this. Vendors are salivating over the opportunities in Kubernetes, Service Meshes, Storage Plugins, Network plugins, and that will be their downfall. Ironically, if all of this were trivial to run, they would still get paid to host/manage it, and perhaps by more people. Amazon gets in a bind: They can’t “manage” a service that doesn’t need management.

If your business model is maintaining that perfect TrapDoor, you’re going to be strip-mined. License be damned.

> The best way to make something toxic for Amazon is to make it so goddamn trivially consumable, that the AWS console and AWS CLI feel like terrible things to ever have to deal with.

On the other hand, making open source easy, simple, consumable and useful will continue to find those who will pay for hosting and management. You will continue to get paid for new feature writing.

Amazon will trip over themselves making it look uglier and stupider with their VPCs and Subnets and IAMs and CloudFormations and what not. That is how you bring Amazon down.
