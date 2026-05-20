---
layout: post
title: "TV writers — champion of Monads"
description: "The most important and overlooked application of Monads in recent history"
date: 2018-03-26 19:09:14 +0000
original_url: https://medium.com/@archisgore/tv-writers-champion-of-monads-ac53beb66077
---

In our zeal for the technical, we frequently miss the artistic. We forget that science is lot more philosophy and art, than it is technical skill. How is it that all talk of monads misses perhaps the most effective wielders of monads in recent history? Television writers, of course!

This post isn’t about what monads are. Instead, I’m going to going to look at why thinking like a story teller makes you a better and effective programmer.

Take any episode of the TMNT, Star Trek (pre-[STD](https://en.wikipedia.org/wiki/Star_Trek:_Discovery)), Three’s company MacGyver, Friends, and many more. You depend on a few things:

### Characters define the show

The premise is the **characters**. Not the story. We might say the shows were “data centric” not “action centric”. It really mattered what was being worked with, rather than what work was being done.

The story, situation, actions these characters find themselves in were secondary.

House isn’t a medical diagnosis documentary. MacGyver isn’t a physics class. What they do is irrelevant. Why they do it is slightly less irrelevant. Who they are, is why we watch it.

![](/assets/images/tv-writers-champion-of-monads/1_XF05MH65bNOSm41WJX5k8Q.jpeg)![](/assets/images/tv-writers-champion-of-monads/1_R3FK4JXNXXu3jBNQ_BsGww.jpeg)![](/assets/images/tv-writers-champion-of-monads/1_OcNK7TQgPGMAFlvV55H-dg.jpeg)![](/assets/images/tv-writers-champion-of-monads/1_6nRXvi3byNOTNz_nxsN72A.jpeg)

### Never let stories change your characters

Ross might get a monkey, but he’s still Ross. Now he’s just “Ross-with-monkey”. It’s been pointed our before, but Ross has two children and they never affect [Ross from being fundamentally Ross](https://www.buzzfeed.com/lmaccarthy/this-friends-theory-could-finally-explain-what-h-a4cp?utm_term=.ixyaP8wdDw#.rkNyaPVwQV).

Let’s look at Ross’s journey here:

![](/assets/images/tv-writers-champion-of-monads/1_k4wzTJTm1gReiU_Aef6gHw.png)

Let’s add some random events here in no particular order and tell me if they feel out of place.

![](/assets/images/tv-writers-champion-of-monads/1_diphYefrygZd-WPcuCAGGQ.png)

I can just as easily believe this progression. We just performed a “composition”.

The trick the writers knew here is that your ***data is paramount***. Operations must bow before data and adapt how they work, to preserve the fundamental structure and identity of the data, never the other way round. You never say, “Ross MUST go on this Everest climb, if that means he will give up drinking, hanging out, and being weird, then too bad.”

And yet… writers get away with some very impactful stories anyway. How do they do it? By constraining side-effects.

### Stories may add side-effects to your characters; safely

Captain Picard gets an artificial heart, but it never comes up when he is [tortured by techniques of Space-1984](https://www.youtube.com/watch?v=wjKQQpPVifY). Or the numerous other times he’s injured, has to rely on Wesley to provide water, trapped in an elevator with kids, etc.

Side-effects are not the problem, so long as you leave the character in a form that the next story can work with without major impact.

![](/assets/images/tv-writers-champion-of-monads/1_2306TUPk7ajwd217No6jRA.png)

“The Picard” has properties and attributes that are inputs to a story. So long as “The Picard”’s properties and attributes are preserved exiting out of your story, such that the next story can work with it, your side-effects are not only allowed, but desirable.

Side-effects give the story itself purpose, meaning and excitement. However, modifying “The Picard” affects input to the next story writer.

Going by the example of Ross above, where this episode fell in the series doesn’t really matter. Every episode before it, and every episode after it would function quite similar. We could add this heart replacement after his Borg assimilation, and it wouldn’t change things one bit!

### What Programmers can learn here

- Your data is paramount. Do not allow operations to change the nature of your data without good cause.
- Add side-effects, add data, add hearts, add monkeys, add children. Do not **change** the character itself.
- Someone gave you that data in a workable format. Be a good citizen, and leave the data after your operation, in a workable format by others downstream.
- Composition wins over coupling every single time. Being able to reorder operations, being able to stream data, being able to express side-effects safely and reliably, will always give you a better system than not.
