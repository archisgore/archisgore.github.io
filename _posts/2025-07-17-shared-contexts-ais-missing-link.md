---
layout: post
title: "Shared Contexts — AIs Missing Link"
description: "A graph of Contexts that can be created, inherited, forked, merged, granted and revoked access to"
date: 2025-07-17 07:15:18 +0000
original_url: https://medium.com/@archisgore/shared-contexts-ais-missing-link-d49e64b187ba
---

A few weeks ago, I prompted a model with a ton of situational context — let’s say I had 200 bananas of 3 different species, arriving on different container ships from 3 different routes, each with different deliver risks. By providing this information, I was able to get my model to create the perfect smoothie plan, which I wanted to share with a colleage. I couldn’t.

In human terms, I wanted to tell my Model, “Can you speak to Mr. Hypokalemia and answer whatever they want based on all this information I’ve given you?”

From there, I wanted the model to answer questions about the perfect smoothie plan, what their potassium consumption would be over time, what they could do to offset risks, and so on.

Even the [typically AI-frenzied LinkedIn](https://www.linkedin.com/posts/archis_ai-hive-mind-two-weird-questions-for-you-activity-7338716918213693442-6c3V?utm_source=share&utm_medium=member_desktop&rcm=ACoAAAEZrskBpq-KZBLgWF47Phcd6GdteQK2kWc) had few satisfying answers.

This rant was written at midnight so it’s unstructured, but I definitely think this should exist. In my head I’m calling it “Confidant.ai” (My personal AI confidant) and I’m calling this method of computing, “Confidant.AI.l computing.”

### Confidant.ai: An AI Network

I’m going to describe a world that someone else should build (because I have [sufficient side-hobbies](https://encrypted-execution.com/) for me to prototype this.) Please skip the “links to context sessions”, or “digital twins” and all that. I know. If those things do what I’m asking, I’m sold on your solution tomorrow — tell me where to sign up.

One of the problems with all models today is they lack global and contextual memories.

#### Use Cases for Confidant.AI-al Information sharing

For example, if I want to share a procedure, a runbook or a cooking recipe, I still have to write/blog/post about it, which other’s models have to scrape to provide them a version for them.

What I cannot do, is go to Claude/ChatGPT/Gemini and TELL it what my recipe is, and then have someone else ask it for “Archis’s recipe” which it can answer for.

This applies to a lot of information — I still enter my calendar information in my calendar tool. I store notes in notes. I store wikis in wikis. Sure AI is an interface to the information, but it could be the Confidant for my information, vending it out to anyone who’d ask, by checking with me.

Imagine someone able to ask Confidant.ai, “I’m thinking of asking Archis to dinner next week. Does he have any preferences or restrictions?”

Depending on who is asking, Confidant.ai would ping me to check if it can share this information with them. Depending on whether or not I grant the permission (through a prompt), it is able to share preferences or decline to share.

Now extend that to calendar tooling — it could remember my preferences for routine activities and answer on my behalf, only checking with me for confirmation.

#### Use Cases for Shared Contexts

Imagine telling Confidant.ai, “I’m thinking of a camping trip with Steve and Tim. Can you reach out to them and figure out the best location for the three of us? For my part, I prefer glamping, with cellphone coverage and a dry place.”

It remembers the shared context, where all 3 of us can seamlessly add to it, and behind the scenes, the AI is constraint solving. But it is only asking questions to each of us where it needs a conflict-resolution or a compromise. This is different than “sharing a link”. If all of us have a valid identity on confidant.ai, we are able to reference each other clearly.

Furthermore, it’s bringing each of our individual personal contexts to weigh in on decisions, while keeping private information discrete. For example, it doesn’t have to tell everyone I’m dairy-free, if there is a viable restaurant that has dairy free options that fits within others’ preferences.

#### Group Shared Contexts

Consider a team, org, company using Slack, Teams, etc. At various grouping levels are ongoing conversations and open questions. My tangential work chat is full of a dozen open questions I know the answers to. These answers are approved-shared (i.e. at least for the group I’m answering them in, the information is public.)

Having the AI “remember” these contexts would immensely help on three fronts:

1. **Preemptive Answers:** Suppose someone asks, “Does anyone remember how to disable ramdisk on a host?” and confidant.ai says, “If this helps, Archis answered this already in various chats. You ssh into the host, and run <foobar>.”
2. **Proactive Tracking of open questions**: I could ask it every morning, “What are some of the questions in all my groups that have been unanswered for more than 3 days?”  
   Furthermore, instead of answering it in the chat, I could tell Confidant.AI, “Ah, so the way to disable ramdisks is…” and it could go respond with that information in any number of places it is relevant. Basically I no longer write Wikis as an out-of-band exercise, but rather I inform my AI, and it adds it to a selectively-shared context.

#### A Context Graph — The Missing Link

I think the key link here is some sort of Context Graph — think Git-for-contexts, or (I cannot believe I’m saying this) a Blockchain for Contexts?A storage system where contexts can be created, inherited, forked, merged, granted, revoked and more.

All these are accessed through a multimodel conversation interface. Rather than think of Passwords, Certificates and ACLs, you think in terms of, “Hey confidant.ai, can you tell me the things about me that are shared with people? And whom are you sharing it with?”

And it could respond with, “Your family knows your blood type, anyone you agree to a meal with can access your dietary preferences, and anyone in the Confidant.AI.l Network can get your answers to the scuba diving sites in the PNW.”

#### We’re Almost There — With Extra Steps

Depending on the reader, this is either solved through something you’re working on — agent personnas, or what not. Or people will never go for this. But people already are!

We’re already in a world where AI’s are talking to AI’s — whereas what we really want is a shared context. Think about:

1. Group Chats — Apple, Google, Meta — all users are using “Rewrite with AI” to write what they want to say. And all readers read AI Summaries. Why the ceremony? I argue that what people really want is to say what they mean, and hear in an actionable format.  
   Maybe I just want to tell Confidant.AI, “Hey I simply don’t want to hang out with so-and-so.” and the person hears the polite version that works well for them. (A tangential rant on how-we-really-speak-different-languages down below.)
2. Resumes and Recruiting — Would be nice for me to tell Confidant.AI my Shor’s algoritm repos, GCC/LLVM modification, and other project repos on Github, my patents on USPTO.gov, and so on. It could remember this context that I could share with places I might be applying to. I already have to use AI to write my resume in a way that the hiring company’s AI can understand it. Why the ceremony where information gets lost anyway?  
   Recruiting companies can simply ask confidant.ai, “Can you find me candidates who can sufficienty solve the discrete log problem on quantum circuits, as demonstrated by code they’ve written that’s distinctly different from typical samples?”
3. Finally News Consumption — twitter, reddit, etc. Why? If you have a thought, why not tell your confidant.ai stuff throughout the day?  
   “You know what I think about Starbucks burnt coffee? I don’t appreciate charcoal-water in the morning!”  
   “I visited Vienna last week. Here’s some photos for you to remember. It’s okay to share these with my friends.”  
   And others could ask, “What are some of the interesting updates from my friends?” or “What are people’s opinions on Starbucks coffee?”

Basically if we’re using AI to produce content and then AI to consume it… what we’re actually doing is: Speaking Different Languages!

Hear me out!

#### An Era of HyperLinguistics

> **None of us speaks the same language!**

My academic passion for cross-language semantic fidelity is established beyond a doubt given that I just spend 8 years developing compilers and interpretors that maintained semantic-meaning across languages.

Here’s the key insight I think we’re all missing, and why decoder-only models might want to consider encoder-decoder architectures: **None of us speaks the same language!**

I know this sounds controversial. Lets construct what Language actually means — it is a symbolic representation of semantic intent. Yet literally every person’s semantic interpretation of the same symbols is dramatically different.

Consider that most of the North American-European world uses the Roman Alphabet. Just because we use the same alphabet doesn’t mean that the letters have the same pronunciation across languages (v, w, j, etc.). Just because the same word is used across multiple languages, doesn’t mean it means the same thing.

So why would two self-proclaimed English speakers mean the same thing, when they use the same symbols? We frequently don’t. Can you recall a time when you had to throw a disclaimer such as, “and I’m using the dictionary definition of this word”, before saying something? Can you recall a time when one person’s ‘request’ meant another person’s ‘command’?

I’m not even touching political discourse with a ten foot pole!

I would argue that the biggest benefit of AI which we’re already using it for, is to say what we mean, and hear what is meant. For some reason we’re all cowardly to eliminate the middle step and build an AI Confidant.
