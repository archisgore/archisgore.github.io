---
layout: post
title: "The Only Winning Move is NOT TO PLAY!"
description: "The Meltdown and Spectre saga continues…"
date: 2018-01-10 17:54:55 +0000
original_url: https://medium.com/@archisgore/the-only-winning-move-is-not-to-play-dfab44fd8e36
---

![](/assets/images/the-only-winning-move-is-not-to-play/1_z3Zbl4lXGtaNkY7h9XfHkQ.png)

#### Setting a Dangerous Moral Hazard

When the Equifax hack happened, or WannaCry happened — we were all helpless. We had no input on protection of our own data. And we all suffered by it. #TaxationWithoutRepresentation

This happened again with Meltdown and Spectre. How many people BECAME insecure between June 2017 and January 2018 because they didn’t know they were buying a device containing a vulnerable chip? Consumers were denied the choice to purchase a product containing a competitor’s CPUs that might have SOLVED this problem. You might argue it really wouldn’t have, but shouldn’t we be able to make those decisions?

Even those who had vulnerable chips, how was disclosure announced? What about all the small operating systems, or embedded code, or forks, or experiments? What about on-prem installations that were not part of the big cloud providers? The big people benefit, the little people wait.

I completely understand the necessity of responsible disclosure. If the flaws had been disclosed to the public without giving Intel, OS vendors and major clouds the time to react and patch, all of us were at risk. All our servers, data, applications — they were ALL vulnerable, at least to some extent. Not that doing this assured us that no malicious person got their hands on the flaws, but it probably helped.

This is a Moral Hazard — much like the housing crisis from a decade ago. Certain things become too big to fail, and there is little choice but to protect them.

#### We Learned Vaporware is Expensive!

My next reaction was the huge expense and cost of reacting to Meltdown and Spectre. Not to an exploit. Not to an attack. But to a vulnerability.

This too seems to be a common theme in cybersecurity. We don’t believe something can be vulnerable. Not until we see it. Then we see it, and we all scramble and react aggressively, costing the world millions.

Although these particular flaws have become a big public deal, this happens ALL the time behind the scenes. A new vulnerability gets announced, and it immediately leads to emergency patching, derailment of development, derailment of releases, derailment of business operations.

Cyberdefense is like [Dr. Strangelove’s Doomsday machine](https://www.youtube.com/watch?v=2yfXgu37iyI). We alternate between two phases. One where we’re completely secure and have nothing to worry about. Alternatively we’re in, “Holy shit they have a doomsday device! Shoot down those bombers!”

#### And Then Security Conflicts With Security….

So this popped into my news feed today: <https://www.bleepingcomputer.com/news/microsoft/microsoft-says-no-more-windows-security-updates-unless-avs-set-a-registry-key/>

Basically, the third-party tool (antivirus software) that you NEED to keep you secure (part of your compliance checklist) now conflicts with the patch you NEED to keep you secure (also part of your compliance checklist).

Which comes first? Does the patch wait? Or does the AV get removed (or in this case updated)?

If you don’t have antivirus, it’s even more comical. You have to manually update a registry key: <https://support.microsoft.com/en-us/help/4072699/january-3-2018-windows-security-updates-and-antivirus-software>.

Twenty years of that painful registry, and nobody bothered to add command-line flags, which would have enabled this:

```
regedit --set --key HKEY_LOCAL_MACHINE --subkey "SOFTWARE\Microsoft\Windows\CurrentVersion\QualityCompat" Value="cadca5fe-87d3-4b96-b7fb-a231484277cc" --type "REG_DWORD” --data "0x00000000”
```

Or you know, something using .Net and objects and Powershell and metadata and XAML because… well, it’s Microsoft.

#### What’s the Moral of All This?

There is one lesson we have apparently never learned: there comes a time when you just have to give up!

Have you ever played Tic-Tac-Toe?

Give up on security? NO! Have you learned nothing?

### The only winning move is NOT TO PLAY!

Today’s game is reactionary. The attacker acts. The defender reacts.

We have to flip this to where the defender acts. And the attacker has to react.

If you didn’t guess the punchline: **DIVERSITY, ENTROPY, SIMPLICITY**!

In one phrase? **Moving Target Defense**.
