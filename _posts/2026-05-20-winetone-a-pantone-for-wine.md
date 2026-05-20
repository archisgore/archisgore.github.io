---
layout: post
title: "WineTone: A Pantone for Wine"
date: 2026-05-20 01:25:47 +0000
original_url: https://medium.com/@archisgore/winetone-a-pantone-for-wine-bf2fd582eaa1
---

Pantone solved color in 1963 by giving every shade a number, and since then no designer has ever had to say “you know, kind of a dusty mauve?” ever again. The conversation ends the moment you pull out the chip.

Wine has had 10,000 years and still can’t tell you what “grippy” means.

Ask a sommelier and they’ll say high tannin polymerization — textural drag on the gums. Ask me, having grown up in India, and “grippy” sounds astringent like water with alum (if you know, you know!) Ask someone who’s never been to France what “French oak” smells like and they’ll stare at you in confusion.

Expressing the same concept individually is one of the most basic joys of life. Yet, every wine recommender on earth, from the free apps to the “Maître Sommelier” interpret your language as if it were universal.

They embed your query into a vector space, search the corpus, and return what *other people* said about wines *they* drank. The system is listening, technically. But it’s hearing you in someone else’s accent.

**WineTone is built to hear you in yours.**

---

#### The Demo That Surprised Even Me

So I did a thing — I pulled publicly available data on 287,000 wine reviews, collapsed into 164,069 distinct (producer, wine, vintage) triples and embedded into 384-dimensional vectors — capturing both what wines taste like and what words humans actually reach for when they taste them.

Ran the query: *“earthy and grippy with jasmine notes.”*

Without any personalization, the system returns a perfectly reasonable American Riesling, a Zinfandel, a Cab Franc — the dictionary neighbors of “earthy” and “grippy,” assembled from what the corpus says those words mean.

Then I tried to calibrate the model TO ME. I labelled six wines I’ve actually drunk, in my own words.

The system now returns four Italian Nebbiolos from Piedmont — including three Barolos it had never been told to favor. From one label on one wine, it inferred that my “grippy” lives in Nebbiolo territory, that my “jasmine” bends toward Barolo’s floral signature, and that the sommalier’s “grippy” and mine are the same word in a different language, with different semantics.

---

#### This Is Not a Wine Problem

The wine domain is just particularly delicious for demonstrating the point, but the underlying failure is everywhere. Someone searching “big cuddly bears” on a pet site probably means Bernese Mountain Dogs, but the system may warn against adopting grizzlies. Someone asking for “cat videos” on a platform that knows they’ve watched five David Attenborough specials is almost certainly looking for snow leopards, not their neighbor’s tabby. Someone asking for “a birthday wine” might want a crowd-pleaser for a dinner party or something rare and weird for a solo celebration — and the right answer depends entirely on *who is asking*, not on the semantic average of everyone who’s ever typed those three words.

The recommender systems that win the next decade won’t be the ones with the most training data. They’ll be the ones that **calibrate the user, not just the corpus.**

---

#### Why It’s Fast, Cheap, and Hallucination-Proof (And Why That’s Rare)

Most AI recommendation architectures make a quietly expensive mistake: they ask the language model to *generate* answers instead of letting the database *select* them. The result is slow, token-hungry, and occasionally confident about wines that don’t exist.

WineTone uses a 33-million-parameter embedding model for exactly one job — turning text into a vector — and then lets CedarDB (a fast, Postgres-wire-compatible analytical database with pgvector built in) handle everything else. Similarity search across 164,069 wines runs in under 50ms. Per-query cost is microcents, not dollars. And because the LLM never produces wine names — every result traces back to a canonical database row with full provenance — there is no hallucination surface. Nothing is being invented. Everything is being found.

The per-user projection that powers personalization is a 384×384 linear matrix, fit via ridge regression in milliseconds and stored in about 250KB. It starts as the identity matrix (meaning a new user gets the generic baseline), and every label they add bends it incrementally toward their personal idiom. The math is closed-form. The cold-start problem effectively doesn’t exist. And the calibration history is versioned, so you can watch each user’s “accent” crystallize over time — which turns out to be a genuinely beautiful thing to look at.

---

#### Where It Goes From Here

The current prototype covers 20,000 of 164,069 wines with dense embeddings (full-corpus encoding on CPU takes 2.7 hours, and I get bored easily). It speaks only English, which is a shame given how much I lecture on distinct languages. And it doesn’t yet incorporate actual chemistry — the full vision in `PLAN.md` pairs the language embeddings with GC-MS chemical fingerprints, which is about $7K–15K of analytical work away from being real.

That last piece is the moat. Language models can learn that people describe Barolo as “tar and roses,” but they can’t smell the rotundone molecule that causes the tar note or the geraniol that causes the rose. A system that fuses natural-language calibration with actual chemical fingerprints isn’t just a better recommender — it’s an objective, versioned, scientific archive of what wine actually *is*, at a molecular level, mapped to what humans actually *say* about it, at a personal level. Pantone for wine, in the most literal possible sense.

The corpus itself becomes more valuable over time as user labels fold back into the training data, meaning tomorrow’s encoder has heard more accents than today’s. That’s a data flywheel with a genuinely interesting shape.

---

#### Memorializing Wines and Reviewers

The wine-industry potential here is two fold and can be extended to other spaces with a subjective-but-internally-consistent grouping. Subjective to individuals, but within the individual, consistent.

First of all, we can memorialize historic wines who have their last 10 bottles on the planet left. Or even rarer. By producing a high-dimensional embedding, if we ever reproduced that wine again, we’d know how far we were from the original. We can also use this to measure vector distances across any arbitrary wines.

Secondly we would memorialize various reviewers. If I have a favorite person whose palette I tend to like more than others, then I can pull up their projection matrix.

---

#### The Thesis

Recommendation systems are very, very good at understanding *the text*. The next leap is understanding *the person*. Not in aggregate — demographic models already do that, and they are, frankly, boring. **Individually. One projection matrix per person. One accent at a time.**

Your “grippy” and my “grippy” are not the same word. WineTone is a small experiment in what becomes possible when a system stops pretending they are.

It’s also, incidentally, a pretty great way to find a Greek Nykteri, Hungarian Furmint or Romanian Dealu Mare — all of which are volcanic, mineraly, floral, but each described differently in wine literature.

---

*Code, architecture plan, and a reproducible demo at* [*github.com/archisgore/WineTone*](https://github.com/archisgore/WineTone)*. Apache-2.0. Built by Archis Gore. Wine-adjacent academics, chemistry labs, and anyone who has ever been handed a Riesling when they asked for something “earthy” — my inbox is open.*
