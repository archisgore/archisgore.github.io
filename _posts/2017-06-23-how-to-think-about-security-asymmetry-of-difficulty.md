---
layout: post
title: "How to think about security: asymmetry of difficulty"
description: "I get a lot of questions from friends and acquaintances, especially non-technical folks, about cybersecurity best practices, what…"
date: 2017-06-23 21:13:15 +0000
original_url: https://medium.com/@archisgore/how-to-think-about-security-asymmetry-of-difficulty-498eeebe91b5
---

I get a lot of questions from friends and acquaintances, especially non-technical folks, about cybersecurity best practices, what technologies they should use, what phones are the best ones, what passwords they should have, etc. The key theme that underlies all these other questions is: “I’ve done everything the internet told me, but how do I *know* that I’m secure?”

The internet is full of rich useful advice. But more than often, there is a big difference between having done everything you can, and intuitively knowing that you are secure. Often times a misconception is that to truly know you are secure, you have to be an expert in cryptography or advanced mathematics. This is most definitely not true.

There is a difference between the first-time baker who is making a cake by strictly following instructions and hoping that the eggs were fluffed enough, vs an experienced baker who intuitively knows that their egg-whites had hard peaks, and their cake isn’t going to be flat and dense. Neither of them needs to be an expert chemist (and even an expert organic chemist, without experience wouldn’t necessarily make the connection.) The inexperienced baker simply doesn’t know what parts of the process are critical and which ones are fungible — because no recipes that I’ve ever come across, give you this information. The experienced baker didn’t learn advanced organic chemistry in culinary school, but they did learn about the fact that stiff peaks on your egg whites are crucial, but the proportion of dry ingredients is somewhat fungible.

In the security context, this leads to a lot of fear and especially confusion around where to spend time/money/effort on, because let’s face it, we all have limited amount of each. When we have a fear about whether a particular phone or server or service is secure enough, we get lots of detailed information on what kind of encryption is used end-to-end, or how certificates work, or how the access controls to the data center work. However it does little to give you that sense of comfort on what any of this means. Is it sufficient? Is it the best they could do? Are there other things they could have done? What else is out there?

This post is intended to introduce a non-technical person on how to think about security. In our cake analogy it is intended to tell you what ingredients and what techniques are crucial, and which ones are fungible. How to get that intuitive understanding of what it is you have done, and why it may or may not prevent a malicious actor from gaining unauthorized access. How to ask the right questions when wondering whether a cloud service is “secure enough” for your data.

The goal here is to try and build a sense of comfort based on an intuitive understanding of what we do, and why we do it — what is it that makes you “secure.”

### Security is Asymmetry

Asymmetry is the fundamental underpinning of ANY security — not only digital security, but physical security as well. Asymmetry is inequality — when a process or method is not the same or not equal for two entities. Ideally one of those entities is “good” (authorized) and another entity is “bad” (unauthorized.)

The very first question you should ask when trying to understand whether something is secure enough for you, is what asymmetry exists between your access to that thing, vs someone who is not you. I’ll address what the asymmetry should be later below, but instead of wondering whether something uses a “Tamper-proof TPM chip”, or “4096-bit encryption”, the first and foremost question you should always get the answer to, is “What is the asymmetry between me and anyone who is not me?”

When Roman warships were lighter and less well build than the Carthaginian ones, there was asymmetry. When Hannibal brought his army through the Alps into Rome, that was asymmetry. When one person has a key to the house, their cost of opening the doors is the application of very little torque to a key. A person without the key must either call a locksmith, have specialized lock-picking knowledge and practice, or apply a larger force to break in. This is asymmetry. The codes to the Enigma machines being known to some people and not others, was asymmetry. In battle, wars, defense and offense — physical or virtual — the primary thing you’re trying to achieve is asymmetry. This is very intuitive when you think about it.

This is a concept that underlies ALL security in ALL contexts, but is something we rarely discuss or address directly unless you’re a researcher. All the best practices, guidelines, products, solutions methodologies and algorithms out there are trying to do one thing first and foremost — increase asymmetry between those who are authorized and those who are unauthorized!

When you’re securing something, the first and in most cases, only, question to ask yourself is, “Is there asymmetry between what I would do to access it, as compared with anyone that is not me?”

On the other hand, it is quite possible to use fancy technology and complex techniques, but add very little asymmetry. Technology by itself does not make things secure. Asymmetry does. This is demonstrated quite frequently in the list of recent cyber attacks. A [recently revealed flaw](https://www.extremetech.com/computing/248919-major-intel-security-flaw-serious-first-thought) that allows machines to be controlled remotely was not a failure of technology. The technology and algorithms were sound, powerful, capable and state-of-the-art. What failed was maintenance of asymmetry. Similarly the leak of [secure boot signing keys](https://arstechnica.com/security/2016/08/microsoft-secure-boot-firmware-snafu-leaks-golden-key/) was, once again, not a flaw in the soundness of cryptography, but a failure to maintain asymmetry.

If you have a laptop with the latest encryption chips, tamper-proof modules, locked hard disks, random-power-drawing CPUs to prevent side-channel attacks, and more all these methods are utterly nullified if there is symmetry of access. If for instance, the laptop were easily physically accessible in an unlocked state for more than one person, then the asymmetry is lost. I believe in this so much so in fact, that there are times where I would advocate using a simpler technique or solution if you can increase and maintain this asymmetry; as compared with a solution that is probably more powerful but where you cannot maintain asymmetry due to circumstances or other limitations.

So remember this easy quick question to decide whether you need something or whether you trust something or someone — “What is the asymmetry between what I, as an authorized user, have to do to access it, vs anyone else — be it the data center operator, the company owner, the NSA, a malicious employee, etc.”

Now note how this is different from asking, “Does a malicious employee have access?” to which you might get an answer “No!” But this answer only speaks to the authorization of the employee — they are definitely not authorized. But reframe that question slightly and ask, “What is the difficulty, as opposed to mine, for a malicious employee to get access?” and you might get a more informative and far more useful answer. As you’ll begin to realize, having a firewall, having a TPM, having a password, having a certificate, having a vault, having GPG, having PKI, etc. do not actually give you any useful information. What you should be looking out for is, what triggers is the firewall biasing against? What keys does the TPM trust — and how is it ensured that only the good guys keep those keys? Who holds the private key behind a cert and how is that protected? So on and so forth… the key is — look for asymmetry created in your favor, and against anyone who is not you.

I touched upon a key topic here that I want to segue into. I spoke about asymmetry, but I didn’t address asymmetry of what exactly. Which leads us to…

### Difficulty, not complexity

If I’ve convinced you that security is a matter of asymmetry, then the thing that should be asymmetrical is *difficulty*. I want to emphasize very heavily that this is not to be confused with *complexity*. The two are neither synonyms nor even remotely similar. Complexity does NO GOOD! Difficult does ALL THE GOOD! If there’s one major take way from this blog, it is knowing the difference between the two, and not let yourself get carried away when complexity masquerades itself as difficulty, which would lead you to a false sense of security.

When you read about cryptography and cryptology, it can sounds very astounding and complex. But in reality, cryptography is remarkably simple. Remember that even the Enigma machine was not a complicated machine. The codes were *difficult* to crack, but the functioning of how it worked was not *complex*.

We will ignore for a moment things like elliptic curve, and start with an example. We can ALL do integer multiplication of arbitrary lengths. And we can all do integer division of arbitrary lengths given a dividend and a divisor. None of this is complicated or complex. It is remarkably simple and is actually something we all learned in middle-school. And that is really all that cryptography is.

Here’s an example to prove it. I am going to communicate a number with you. This number is the nuclear launch code for a toy nerf gun missile I have, but I have “encrypted” this number: *9810737418200856*.

I will even disclose the process by which this number is encrypted. I literally used middle-school on-paper multiplication to multiply my secret nuclear code with a very large prime number. If you know the prime (which I will share with you privately), you can divide it trivially by hiring a 5th grader, and you’ll get the launch code. If you don’t have the prime, you’ll have to factorize the number I gave you until you get all the factors, and perhaps you’ll know which one of those primes was my “encryption key” and then the remaining factor is the launch code.

If you’re paying attention, none of the operations above are very complicated, magical, or require an understanding of more than basic arithmetic. Even though this is a very basic example, you’ll find it very difficult to guess what my launch code is. None of this is particularly complex though. I tried a quick google search on prime factorizers, and none of the ones on the first page gave me the result.

I’ll tell you what my prime number is: *179426549.*

Given this information it should be trivial to divide *9810737418200856* by *179426549*and you’ll get the launch code.

What you just saw is an example of quite legitimate cryptography. Nothing about it was wrong or missing, save for the fact that my prime number was very small. You can probably think of intuitively making this “stronger” by which I mean increasing the difficult of figuring out the factors.

This falls under a class of problems in mathematics and computational science which are very difficult to solve, but if you are proposed any single solution, it is trivial to verify if the proposed solution is in fact a valid one or not.

Going to other examples — think of the innocuous and popular captcha. It is not complex to setup or understand, but it is very difficult (computationally expensive) if you are a machine, and almost trivial if you are a person. See the massive asymmetry here?

Security is founded on difficult problems, not complex problems. So the second question you should ask when evaluating a product, service, cloud, person, etc. is “How difficult is it for me to solve this problem, and how difficult is it for someone else to solve the same problem, and why?”

Passwords are “difficult” because you have to go through ever single possible permutation. Captchas are difficult because machines find it difficult to interpret fuzzy characters. Encryption is difficult because prime-factorization is expensive. None of these are complex.

Complexity is something that is burdensome, but not necessarily difficult. I think of it as equivalent to being Kafkaesque. Injecting a backdoor for instance, is today a complex problem. You have to get hired in the company, write some legitimate code for them, build relationships with the code reviewers, and get them to approve the submission, and get that code checked in. But this is not a difficult problem. The expense of solving it is still tractable. Similarly, if accessing a datacenter’s hard disk has a lot of paperwork involved, and multiple approvals involved, it might be a very complex thing to do, but not a very difficult one. Writing malware to take advantage of a buffer overflow is complex — it requires knowledge of machine code, compilers, layouts, etc. It might be incredibly frustrating. But it is not a difficult thing to do.

In the physical world, if you are checked into a hotel, think about the “difficulty” of accessing your hotel room. I don’t think it is difficult at all — there are probably hundreds of cleaning people, security people, and more who have access to it. It might be complex to gain their trust, but the overall process of getting in is not difficult.

### Measuring and quantifying Security

I want to conclude this post by noting that I think the cyber security is going through the same pragmatic revolution that the operations world went through, over the last two decades, and I think it is inevitable.

Way back in the day, before I went to college, I remember these million dollar “mission critical” machines people would advertise in computer magazines. They were extremely complicated boxes with fully parallel computers running in the same box with parallel redundant power supplies, and redundant UPSes and redundant generators with hot standby. Each processor would execute the exact same instruction simultaneously and should there be a binary error in one of them, the hardware would fall over.

The thought was that once you got this “perfect machine” all you had to do was write straightforward trivial code, and you are done! The machine never goes down. You as a programmer never worry about it. If you had to sell a piece of software that was mission critical, you only had to encourage the purchase of one of these machines.

At some point, due to physical necessity, Google came along and decided that that wasn’t going to work for them. What mattered is the overall uptime of Search, not the uptime of “a machine”. It would still be a full two decades (from when I was in high school through today) that we learned that what matters is the “overall experience” not “individual components”. Rather than try to make disks failure-proof, you have to make data failure-proof. For Erlang fanboys, this should be no news at all — Joe Armstrong figured this out in the early ’80s.

In the style of how we note algorithmic complexity, we note the probability of failure using a big-P notation. For uptime instead of looking at the uptime of one disk or one memory array or one CPU, we look at what matters most — The probability of a piece of “data” or “functionality” being up. We now look at P(“airline bookings”) = 99.9999999. Meaning that the probability of “airline bookings” being up at any given time is 99.9999999.

In the security world, we are beginning to move over to such metrics rapidly, because we are facing the same problems Google faced around operations in the late-90’s. When a breach happens, it doesn’t really matter whether a TPM chip failed, or a buffer was not bounds-checked, or someone left their password written down, or a private key was leaked.

What really matters is the P(“functionality”). What is the conditional probability that, assuming individual features will fail eventually, the overall data is protected?

This is what I call the Big-Oh for Security. We are increasingly, as an industry, being backed into a corner, where we have to think about and plan for individual failures, restarts, upgrades, etc. and account for those when looking at the big picture, which is protection of the data we value most.

And for that — we need to be asymmetrically difficult. Easy for the “right” person. Difficult for the “wrong” person. And an asymmetry that is as unfair as we can make it.
