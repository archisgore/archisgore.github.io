---
layout: post
title: "Assume good intentions"
description: "The assumption of malice leads to an unproductive response"
date: 2018-08-29 19:35:30 +0000
original_url: https://medium.com/@archisgore/assume-good-intentions-4e28406ef84d
---

At my very first job in the industry, I consistently submitted bad code reviews. I formatted my code all wrong. The company even had a code-format-czar to ensure people like me didn’t mess up the code base.

> “Archis doesn’t care about good code.”

In my second job, some genius came up with automation to blame me. It was a static analysis tool that would throw errors and warnings on all of my formatting mistakes. It was humiliating to be blamed by a robot. The pinnacle of human ingenuity, decades of investment in AI, machine learning, static analysis, compilers, parsers, and tokenizers were being used to blame me instead of changing the accidental usage of a tab instead of spaces.

> “Archis still doesn’t care about good code. Fortunately, we have AI to tell us when he breaks it.”

Three years ago, perhaps the most under-appreciated genius in all of programming history from Google, gave me: `go fmt`. I run it every single time before I commit. This ensures that we have zero unformatted code in our codebase.

> “Archis….. umm…. runs go fmt every single time I guess. Anyway it doesn’t matter because we run it before submission.”

Was my intent to maliciously try to get tabs in under the radar? My first job definitely thought so. Their reviewers even prided themselves on catching me. Heck they wrote fancy tools to show placeholders for tabs and spaces just so the reviewer could easily see the character. My second job also assumed malicious intent, but decided a code reviewer wasn’t even necessary to point it out; AI could do it.

My current job assumes my intent is always to meet the reviewer’s standards, and channel all that fancy Computer Science cleverness in a tool to help me **execute** my intent. We have no un-formatted code, and if we do, we have a script that will go fix it 100% of the time. No exceptions. [We don’t wonder ‘what if’, we have an answer for ‘what then’.](https://blog.polyverse.io/threat-models-suck-100ab95db8f5)

### Cybersecurity has a problem: Nobody cares about us

Today morning, I read this article from BlackHat: <https://www.technologyreview.com/s/611727/cybersecuritys-insidious-new-threat-workforce-stress/>

Slashdot picked it up: <https://it.slashdot.org/story/18/08/08/1517245/cybersecuritys-insidious-new-threat-workforce-stress?utm_source=feedly1.0mainlinkanon&utm_medium=feed>

According to the article, cybersecurity teams are burning out having to deal with all the problems coming their way. But what is the real problem? If you take a look at the Slashdot comments, the problem is a predictable one:

1. Businesses don’t care about security. They only care about features.
2. We told developers not to bump versions, but they did it anyway.
3. Developers never used hardened binaries when we asked them.
4. Nobody built a standardized authenticated API despite us telling them to.

On the face of it, the conclusion seems to make sense:

“I wanted an authenticated API” => “I commented on design review that they should do it” => “I never saw it done” => “They don’t care about an authenticated API” => “I stopped attending design meetings, because nobody cares about security”

This is either a very childish passive-aggressive conclusion to try and get a developer to say, “*Awww… I’m sorry. I didn’t mean that. I really do care about security and all of your opinions. I’m sorry I ignored you. Let’s talk for two hours tomorrow and you can tell me all about your certs.*”

Or it is assumed that nobody really cares. In which case our reactions are all the more dangerous.

### There’s a better way

Two years ago, one of my colleagues sent me this:

```
docker run -e SSL_SUBJECT="example.com" paulczar/omgwtfssl
```

Today, I ALWAYS generate fresh certs. A lot of them. Literally on every container deploy, even locally.

This wasn’t the case two years ago. Back then, I had a set of three cert directories on my laptop:

```
ls ~/certs  
self-signed-test  
self-signed-internal-prod  
ca-signed-prod-<website>
```

Did I suddenly begin caring about security? Did I have a change of heart? Was it some traumatic experience?

The narrative was actually much simpler: I always wanted certs. I wanted to tell a robot, “Hey you! Give me a fresh pair of certs.”, and when it finally began doing that, I started to use them.

What I didn’t want to do is generate them by hand every time, because my job **begins** at generating the certs, and ends at **deploying the product**.

Business Leaders, Product Owners and Developers don’t have malicious intent. Developers care about security. They care a great deal. They became developers so they could get machines to do the jobs they didn’t want to do. That is why they build AI, robots and automation. Even more so, Business Leaders entire business depends on security.

Accusing someone of not caring, in hopes of getting a reaction, is a method that stopped working for me roughly around the age of 17. Maybe I pulled it off a couple of times in my early 20’s but past that, it lost all impact.

### Provide means to execute intent, not question it

Throughout my entire life I’ve seen that by assuming good intentions and then giving people the means to execute on them, leads to exceptional results every single time.

The change in attitude, expectations, tools and procedures is drastically different based on whether I assume a developer has good intent or malicious intent. Here’s two scenarios, you can pick the one you want to live in.

#### When I assume malicious intent

1. I look for ways to question intent, look for places the developer will screw up, and add alerts to notify me.
2. My use of tools is designed to automate MY job — which is solely questioning, calling out and notifying.
3. My job ends when I’ve called someone out and proven they don’t care about security. A narrative I continue to reinforce.
4. When they see my next big tool, they think, “Oh great. Instead of Archis poking at my code reviews, he’s got a robot to do it.”
5. I’m screaming at everyone to do this, I even automated my screaming — using Machine Learning, Analytics, AI, Data, Dashboards, alerts, etc. I am screaming but no one is doing anything about it. Thus I conclude nobody cares about security; and consequently, me.

This is a fairly dysfunctional and dystopian way to live.

#### Change that to assumption of good intent

1. I look for ways to make not-screwing-up the default workflow.
2. My job begins when I notice they’re executing wrong, and it ends when they have to do nothing to execute right, but **take proactive action** to execute wrong.
3. They see my new tool and think, “Holy cow… I no longer have to generate certs every time I deploy. Here’s AI and automation that’ll do it for me. It’ll get it right every single time.”
4. Everyone’s screaming at me to automate more, to do more, to be involved more. The more I’m involved, the more secure everything becomes, and the less work everyone else has to do.

I’d much rather live in the second world, wouldn’t you?
