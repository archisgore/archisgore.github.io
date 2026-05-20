---
layout: post
title: "The Docless Manifesto"
description: "“undocumented” code is the new ideal"
date: 2018-12-10 00:05:53 +0000
original_url: https://medium.com/@archisgore/the-docless-manifesto-3d81c0450a54
---

Documentation is necessary. It’s essential. It’s valuable. For all its purported benefits though, even towards the end of 2018, it remains relegated to a **stringly-typed escape-hatch to English, at best**.

Let’s dissect that problem statement below, but feel free to skip the section if you nodded in agreement.

### The Problem: Documentation is a Stringly-Typed escape hatch to English (at best)

#### [String-Typing](http://wiki.c2.com/?StringlyTyped): A performance-review-safe way of saying “no typing”.

String-typing is the result of two constraints placed on developers:

1. We must be strongly typed; we’re not hipsters — we enforce strong opinions on consumers of our program/library/platform.
2. We must be extensible; we don’t know if we have the right opinion necessarily, and are afraid of getting abstracted out or replaced if we end up being wrong.

So how do you have an **opinion** that can **never be wrong**? You “enforce” the most generic opinion humanly possible:

```
// C/C++  
(void *) compare(void *a, void *b)
```

```
// Java - but hey, at least primitive types are ruled out!  
static Object UnnecessaryNounHolderForFunction compare(Object a, Object b)
```

```
// Golang  
func compare(a interface{}, b interface{}) (interface{}, error)
```

There it is! Now we can never have a wrong opinion, but gosh darnit, we enforce the opinion we have — don’t you be sending stuff the programming language doesn’t support! We will not stand barbarism.

#### **How does anyone know what those functions do?**

We can solve this problem in one of two ways:

1. You can just read the source code and stay up-to-date on what it does at any given iteration (and make the code easy to read/comprehend.)
2. Or we can double-down on how we got here in the first place: If we had a highly opinionated but extensible way to document what it does, we could document anything: Welcome to **Code Comment Documentation**!

The strong opinion: You can add a string of bytes (non-bytes will NOT be tolerated; we’re not barbaric), that can represent anything (extensible). It is left to the user to interpret or trust anything said there — ASCII, Unicode, Roman Script, Devnagri, English, French, Deutch, whatever — it’s extensible, but at least it’s all bytes. We won’t parse it, store it, matain it, or make any sense of it in the compiler — we will contractually ignore it.

#### Why we don’t program in English in the first place?

Past the low-hanging fruit about English (or any natural language) being specific to a region, culture, or people, there is a much deeper reason English is not used to write computer programs: English is semantically ambiguous.

It is remarkably difficult to get two people, let alone a machine, to make sense of an English statement in the exact same way. Which is why we build programming languages that define strict semantics for every statement.

So how does a program written in a deterministic programming language whose meaning can’t be made sense of, become easier to comprehend when rewritten in the form of a code comment document in a language that fundamentally cannot convey accurate meaning? It doesn’t, of course. But much like [CPR, which works far less than most people think](https://healthcare.utah.edu/the-scope/shows.php?shows=0_wz98zi6n), it is a form of therapy for the developer to feel like they did everything they could.

A second problem, that annoys me personally, is the blatant violation of DRY (Don’t Repeat Yourself.) You write code twice — once in your programming language, and once again in English (at best — because that’s all I read.) I have to usually read both if I have to make any reliable bet on your system. Only one of those two are understood unambiguously by the computer, you and me. The other one is not even understood by the computer, while you and I may not interpret it in the same way.

### We can do better!

We have the benefit of hindsight. We have seen various constructs across programming languages. We can and should do better!

1. Unit tests are spec and examples — they demonstrate where inputs come from, how to mock them, and what side effects should or should not happen. A Unit-Test becomes at once, a usage document, but also an assurance document.
2. Better type-systems allow us to capture what we accept.
3. Function/Parameter annotations can provide enumerable information to a caller.
4. Function Clauses and Pattern Matching allows the same function to be better expressed in smaller chunks with limited context.
5. Guards can merge input-validation and corresponding “what-I-don’t-accept” code comments into one — a machine-parsable enforceable spec + a beautiful document to read.
6. Documentation as a first-class part of language grammar (that homoiconic influence from Lisp again.) In at least one system in Polyverse, we made code-comments enumerable through an interface (i.e. `object.GetUsage()` returns usage info, `object.GetDefaultValue()` returns a default value, etc. etc.) This means we can query a running compiled system for it’s documentation. We no longer have version-sync problems across code, or fear of loss of documentation. If all we have left is an executable, we can ask it to enumerate it’s own public structs, methods, fields, types, etc. and recreate a full doc for that executable.

All of this information is available to us. We intentionally lose it. It annoys me every day that I read someone else’s code. What else did they want to say? What else did they want to tell me? How is it that I force them to only communicate with cave paintings, and blame them for not explaining it better?

This led me to write:

### The Docless Manifesto

A **Docless Architecture** strives to remove arbitrary duplicate string-based disjoint meta-programming in favor of a system that is self-describing — aka, it strives to not **require documentation**, in the conventional way we understand it.

The Docless Manifesto has two primary directives: **Convey Intent** and **Never Lose Information**.

#### Convey Intent

The question to ask when writing any function, program or configuration, is whether you are conveying intent, more so than getting the job done. If you convey intent, others can finish the job. If you only do the job, others can’t necessarily extract intent. Obviously, when possible, accomplish both, but when impossible, bias towards conveying intent above getting the job done.

Use the following tools whenever available, and bias towards building the tool when given a choice.

1. **Exploit Contentions**: If you want someone to run `make`, give them a Makefile. If you want someone to run `npm`, give them a `package.json`. Convey the obvious using decades of well-known short-hand. Use it to every advantage possible.
2. **Exploit Types, Pattern-Matching, Guards, Immutable types**: If you don’t want negative numbers, and your language supports it, accept unsigned values. If you won’t modify inputs in your function, ask for a first-class immutable type. If you May return a value, return a `Maybe<Type>`. Ask your language designers for more primitives to express intent (or a mechanism to add your own.)
3. **Make Discovery a requirement**: Capture information in language-preserved constructs. Have a `Documented` interface that might support methods `GetUsage`, `GetDefaultValue`, etc. The more information is in-language, the more you can build tools around it. More so, you ensure it is available in the program, not along-side the program. It is available at development-time, compile time, runtime, debug-time, etc. (yes, imagine your debugger looking for implementation of `Documented` and pulling usage docs for you real-time from a running executable.)
4. **Create new constructs when necessary**: A “type” is more than simply a container or a struct or a class. It is information. If you want a non-nullable object, create a new type or alias. Create enumerations. Create functions. Breaking code out is much less about cyclomatic complexity and more about conveying intent.  
   A great example of a created-construct is the *Builder Pattern*. At the fancy upper-end you can provide a full decision-tree purely through discoverable and compile-time checked interfaces.
5. **Convey Usage through Spec Unit-Tests, not Readme**: The best libraries I’ve ever consumed, gave me unit tests for example usage (or not usage.) It leads to three benefits all at once. First, I know that it is perfectly in sync without guessing, if the tests pass (discoverability), and the developer knows where they must update examples as well (more discoverability). Secondly, it allows me to test MY assumptions by extending those tests. The third and most important benefit of all: Your Spec is in Code, and you are ALWAYS meeting your Spec. There is no state where you have a Spec, separate and out of sync, from the Code.

#### **Never Lose Information**

This is the defining commandment of my Manifesto!

Violating this commandment borders on the criminal in my eyes. Building tools that violate this commandment will annoy me.

The single largest problem with programming isn’t that we don’t have information. We have a LOT of it. The problem is that we knowingly lose it because we don’t need it immediately. Then we react by adding other clumsy methods around this fundamentally flawed decision.

My current favorite annoyance is code-generation. Personally I’d prefer a 2-pass compiler where code is generated at compile-time, but language providers don’t like it. Fine, I’ll live with it. What bugs me is the loss of the semantically rich spec in the process. It probably contained intent I would benefit from as a consumer.

Consider a function that only operates on integers between 5 and 10.

When information is captured correctly, you can generate beautiful documents from it, run static analysis, do fuzz testing, unambiguously read it, and just do a lot more — from ONE definition.

```
typedef FuncDomain where FuncDomain in int and 5 <= FuncDomain <= 10.
```

```
function CalculateStuff(input: FuncDomain) output {  
}
```

When information is still captured for you (in English) and the programming language (through input validation), you lose a lot of the above, and take the additional burden unit-testing the limits.

```
// Make sure input is between 5 and 10  
function CalculateStuff(input: int) output {  
    if (input < 5 OR input >= 10) {  
        // throw error, panic, exit, crash, blame caller,   
        // shame them for not reading documentation  
    }  
}
```

This borders on criminal. What happens when you pass it a `5`? Aha, we have a half-open interval. Documentation bug?`Make sure input is between 5 and 10, 5 exclusive, 10 inclusive. Or (5,10]`. We just wrote a fairly accurate program. It’s a pity it wasn’t THE program.

We can certainly blame the programmer for not writing the input validation code twice. Or we may blame the consumer for having trusted the doc blindly (which seems to be like the [Pirates Code… more what you call a guideline](https://www.youtube.com/watch?v=WJVBvvS57j0).) The root problem? We LOST information we shouldn’t have — by crippling the developer! Then we shamed them for good measure.

Finally, the absolute worst, is where you have no input validation but only code comments explaining what you can and cannot do.

I could write books on unforgivable information-loss. We do this a lot more than you’d think. If I was wasn’t on the sidelines, Kubernetes would hit this for me: You begin with strongly-typed structs, code-generated from spec because the language refuses to give you generics or immutable structures, communicated through a stringly-typed API (for extensiblility), giving birth to an entire cottage industry of side-channel documentation systems.

In a world where information should be ADDED and ENRICHED as you move up the stack, you face the opposite. You begin with remarkably rich specs and intentions, and you end up sending strings and seeing what happens.

So here’s my call to **Docless**! Ask yourself two questions before throwing some string of bytes at me and call it “Documentation”.

1. Have you taken every effort to convey intent?
2. Have you missed an opportunity to store/capture/expose information that you already had before duplicating the same thing in a different format?
