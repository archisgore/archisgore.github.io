---
layout: post
title: "Learning from Machines — Part 1: Types of Problems"
description: "Can we apply Machine Learning to our day-to-day lives?"
date: 2019-04-04 21:37:39 +0000
original_url: https://medium.com/@archisgore/learning-from-machines-part-1-types-of-problems-c4b8ce74bfc8
---

![](/assets/images/learning-from-machines-part-1-types-of-problems/0_NIy9Q-aoRyHIdWsq.jpg)

Much is made of AI, ML, Deep Learning, Neural Networks, etc. today, and a lot of it can be mystifying. With these series of posts, I hope to de-mystify ML by re-discovering how ML techniques can help us in our regular fleshy lives; where these techniques originated from in the first place, in some cases over two thousand years ago. Let us call this discipline LM (**L**earning from **M**achines.)

Why bother? Why do we care? Unless you have achieved [Enlightenment](https://en.wikipedia.org/wiki/Enlightenment_in_Buddhism), you have problems. I certainly do. We think, we act and we bother because we want to solve problems that are very personal to us.

#### Problems! Problems! Problems!

> 99% of the battle for efficiently solving life’s problems is to identify what fundamental category it fits in.

Everyone has problems. School, education, finances, wants, needs, desires, fear, control, anxiety, etc. Problems. Problems. Problems. We want to skate on ice with minimum friction, but also want to stop with extreme friction. We want maximum money for minimum work. We want “some kind of Thai food”, but not a particular restaurant in mind. What should we do about the traffic ticket? How do we know our doctor is prescribing the right stuff? How do I know my [coffee won’t poison me](https://blog.polyverse.io/threat-models-suck-100ab95db8f5)?

The fundamental trick machines exploit to solve problems, sometimes better than us, is knowing that not all problems are the same. They force us to categorize our problems, and then they use the best tools for the job. 99% of the battle for efficiently solving life’s problems is to identify what fundamental category it fits in.

As you read the problems, I want you to train your mind to completely give up any attempt at “solving” them instinctively. This won’t be easy, and it takes time learn this skill. Try not to relate to it or day-dream of what *you* would do when faced with one. This skill is valuable in and of itself — when listening to customers, when reading news, when learning a new technology, when reading an opinion. This is the one advantage machines have over us — they don’t feel uncomfortable over a lot of unsolved problems facing them.

### Three Fundamental Problem Types

#### Type I: Where you know the exact solution

This is the easiest problem to solve — because it is already solved; intuitively seems to be the most common, but often mistakenly so. This is the rarest of all problems. Even when it exists, it usually comes with tolerances. You may want **chocolate cake** but that comes with a wide variety of tolerances on ingredients, techniques, styles, etc. While rare, this problem does show up: you may want one specific house, or one specific car.

#### Type II: Where you will know the solution when you see it

This is the slightly more difficult problem to solve. This problem does show up a lot — when you shop for clothes or fabrics, or even if you’re browsing for a car. You don’t know what you want, but if you saw the perfect product, you’d know it at an instant.

This problem has two sub-categories:

1. **With directionless feedback**: This is where you just say “Yes/No” to every choice. This would be like that terrible teacher you had who simply tells you you’re wrong and asks you to redo the work, or the bad client who just says that’s not what they wanted but adds little more by way of desire or expectation.
2. **With directional feedback**: This is where you say “No, because the fabric is too stretchy.” This subcategory of problem is what **Artificial** **Neural Networks** solve — using directional feedback, they course-correct each choice, until you tell them you’ve found what you wanted.

This problem is also deceptively uncommon. You may have easily mistaken clothes shopping as falling under this category because once you’ve seen all the options, you may know what you want. But that’s not what the title says — it says when you see an item of clothing, you have to know specifically at THAT item, whether it is the first or the last, that it was the solution.

Good examples of this category are:

1. In Chess, if you see a checkmate, you want for nothing more. You don’t necessarily know how to checkmate, but when you see the checkmate, you know it’s what you want. Whether it’s the first move or last move, if you see it, you need no more options.
2. In positive identification in a lineup — once you see the person you want to identify, whether first or last, you need no more options. You don’t know the exact person before the lineup, but once you see them, you know you need to see no more options.

#### Type III: Where you don’t know the solution even if you saw it

> **We’re really looking for the best of the worst**

This is by far the hardest problem to solve, and also the most common. This is also the cause of anxiety, stress, indecision, and the like.

Most of us don’t really know what we want directly, and we won’t know it if we see it. What we do know is that once we’ve seen every possible option, we will choose the best. By doing that, **we’re really looking for the best of the worst.**

Most unbounded decision problems fall under this category — how do you select your doctor? How do you hire an employee? How do you find a lawyer? How do you know what clothes to buy? Computationally they are the [Traveling Salesman Problem](https://en.wikipedia.org/wiki/Travelling_salesman_problem).

This is the problem that leaves us with great anxiety — did we pick the Best Lawyer? What if we could have done better? Are we missing out? Did I get swindled? Did I get sub-optimal? So much fear, anxiety, and stress!

This is where you may have seen the best lawyer in town… but seeing that specific lawyer doesn’t tell you that is who you want. In the best case you may need to see a LOT of lawyers, and ideally ALL lawyers before you know you’d seen the best. However, if you’re not an expert in judging lawyers, even after looking through them all you may not know who was the best because you don’t know “how” to judge the best.

---

Let’s not solve any of these problems today. This is only one dimension to categorize the problems in, but in my opinion the most important one. Any time you feel indecisive or anxious, see if you can’t identify what Type of problem you’re facing?

Note that the impact or importance of the problem has nothing to do with the Type it is in. Buying a house, perhaps the single most impactful, risky and important decision you may ever make, can frequently fall under **Type I**. Treating a chronic or high-terminal-risk illness can frequently fall under **Type I**.

On the other hand, choosing the best dress to wear for a party can easily fall under **Type III.**

Our brains trick us into categorizing problems based on importance. We think by thinking really really hard, or by reiterating just how important something is, we are driving toward a solution. “I don’t know much about Homelessness, but it is important to me damnit! I care!”

Don’t confuse [difficulty with complexity](https://blog.polyverse.io/how-to-think-about-security-asymmetry-of-difficulty-498eeebe91b5).
