---
layout: post
title: "Fun with binaries!"
description: "ASLR and DEP defeated with three instructions and one offset!"
date: 2017-11-22 00:25:28 +0000
original_url: https://medium.com/@archisgore/fun-with-binaries-4361128556a4
---

ASLR and DEP defeated with three instructions and one offset!

![](/assets/images/fun-with-binaries/1_G7sdZ04WWFr1ULdbSqE8vw.png)

This is Part 2 of my [previous post](https://medium.com/@archisgore/lets-craft-some-real-attacks-8efac7b3df90) that demonstrated how you craft undetectable attacks against binaries, using our colorful Open Source [Entropy Visualization](https://analyze.polyverse.com) tool. I left you with a cliffhanger… so let’s begin there!

#### Recap of the cliffhanger

The cliffhanger I left you with was that all we need are three tiny ROP gadgets, and the offset of [mprotect](http://www.informit.com/articles/article.aspx?p=23618&seqNum=10), to make any arbitrary part memory executable. First, I present my proof:

I pitched heavily to get a [Captain Picard](https://www.youtube.com/watch?v=Jph2qWXJ-Tk&t=124s) impersonator, but I got shot down.

This is a video by Roy Sundahl, one of our most senior engineers, and our resident ROP expert who spends a lot of his time figuring out offensive tools.

Before we proceed, if you’re wondering why we can’t just block calls to mprotect, it turns out there’s some truth to [Greenspun’s tenth rule](https://en.wikipedia.org/wiki/Greenspun%27s_tenth_rule). Let’s forgo the obvious candidates like interpreters and JITers. I learned that the tiniest of programs that might use regular expressions will need to call mprotect — including the innocuous “[ls](https://en.wikipedia.org/wiki/Ls)”.

#### Let’s cast a wider net!

Okay that exploit was cool, and you can do this for yourself by finding gadgets across all the libc’s in the samples.

But can we do more? Can we easily go after a range of machines \*without\* knowing a target signature? Let’s find out!

![](/assets/images/fun-with-binaries/1_9XSWxvWAgi69-HuRYRse0Q.png)

Here I’m comparing the same “version” of libc across CentOS 7.1 and 7.2. For a quick reference, on the right, rows with a red background are gadgets that survived perfectly, yellow background are gadgets that exist but at a different location, and no background are gadgets that didn’t exist in the first file.

We found some 2503 gadgets across them. You notice how little variation there is when the code was compiled at two different times, from what is probably two variations. The more gadgets that fall on the same addresses, the easier it is for us to cast a wide net since it requires that many fewer custom craftings to go after a binary. The way to determine if your exploit will work across both, first filter the right side by “Surviving Gadgets”, and then search for gadgets you want.

Let’s try that across CentOS 7.1 and 7.2. First up, `pop rdi ; ret`? Yep! There it is! The first common address is: `c6169`.

![](/assets/images/fun-with-binaries/1_W7sLxc2n1WlbijJIHg3opQ.png)

Second up, `pop rsi ; ret`? Yep! There it is also! First common address is: `c7466`.

![](/assets/images/fun-with-binaries/1_qmDeh1DWK91joj4H5vUDwg.png)

Finally, `pop rdx ; ret`? Yep! The first surviving address is: `1b92`.

![](/assets/images/fun-with-binaries/1_8XCvAXA6HCPVBZFNwlBi_w.png)

We got our complete ROP chain across both binaries: `c6169` `c7466` `1b92`. We can validate this by simulating execution across both binaries.

![](/assets/images/fun-with-binaries/1_2EI2kyfQtRNqD-fMXWm3sg.png)

Now you know the complete power of the tool!

This is what the tool is intended to do! You can verify rop chains across binaries without ever leaving your browser. You can now tell, visually and graphically, whether a particular attack will work against a given binary you run. It can be used to craft attacks, but it can also be used to ensure that a patch really worked.

There’s a bit of emotional comfort when you can execute a chain visually, see how the flow jumps around, and see that it doesn’t work.

#### Are Overflows/Leaks that common?

All this depends of course, on you being able to manipulate some little bit of stack space. Aren’t overflows so…. 2000s? We use bounds-checked modern languages that don’t suffer from these problems.

First of all, if you subscribe to our [weekly breach reports](https://blog.polyverse.io/weekly-breach-report-2fbd77c25087), you’ll empirically find that overflows and memory leaks are pretty common. Even the internet’s favorite language, Javascript, [is not immune](https://www.google.com/search?q=buffer+overflow+javascript&oq=buffer+overflow+javasc&aqs=chrome.0.0j69i57.4255j0j7&sourceid=chrome&ie=UTF-8).

Secondly, my best metric to find truth is to look for back-pressure (the sociological version of [proof-by-contradiction](https://en.wikipedia.org/wiki/Proof_by_contradiction)). Look out for attempts at locking this down 100%, and then [follow the backlash](https://www.reddit.com/r/rust/comments/2qxid1/rust_in_practice_is_unsafe/).

However, I also want you to get an intuitive understanding of where they arise and why they happen.

Even I have to admit that certain operations (such as sorting or XML/JSON parsing) are better implemented by manipulating memory buffers directly, despite my well-publicized extremist views favoring immutable data and list comprehensions,

So what does a “real overflow” look like? (Code in the samples directory.)

```
#include <stdio.h>
```

```
#define BUF_LEN 20
```

```
int main()  
{  
    char buf[BUF_LEN];  
    int i=0;
```

```
    while (i++ < BUF_LEN) {  
        printf("Setting buf[%d] to zero. \n",i);  
        buf[i] = 0;  
    }  
}
```

I just overwrote a byte on the stack frame. It’s obvious when I point it out. If you were working on this code and not looking for overruns, this is easy to miss. Ever seen the college textbook example of a quicksort using while-loops to avoid using the system stack? They are liberal with `while(1)`s all over the place.

> Personal Rant: They are very common, and they are insanely difficult to find. This is why I’m such an extremist about immutability, list comprehensions, symbolic computation. For your business apps, you should NEVER, unless under extreme exceptions, listen to that “clever” developer who is doing you the favor of writing efficient code. Pat them on the back. Give them a promotion or whatever. Get them out of the way. Then find a lazy person who’ll use list-comprehensions and copy-on-change wherever possible! I’m a big believer in Joe Armstrong’s advice here: First make it work. Then make it beautiful. Finally, if necessary, make it fast.

In our analyses, more than 65% of critical CVEs since June 1st fell under this category. I could be off by a few points on that number since it changes as we compile our reports periodically and tweak how we classify them. But it’s well over 60%.

#### Putting it all together

In Part 1, I showed you what ROP gadgets are, how to find them, chain them, and exploit them.

In Part 2, I completed the story by demonstrating how to find common gadgets across a wide array of deployed binaries.

The purpose of the Entropy Visualizer is to enable all this decomposition in your browser. In fact this is an easier tool than most ROP finders I know. :-)

Happy Hunting!
