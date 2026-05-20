---
layout: post
title: "A love-letter to Kubernetes"
description: "Reduce cognitive load, have opinions, and advocate for the App Developer not the platform vendor"
date: 2018-04-10 05:24:20 +0000
original_url: https://medium.com/@archisgore/a-love-letter-to-kubernetes-32675736097b
---

I needed to get this off my chest. I know it is career suicide to say Kubernetes is anything short of the best architected magical can-do-no-wrong platform of today.

Kubernetes rubbed me the wrong way when I first saw it because I couldn’t run it, I went through a brief consolation period when the compliance program was launched, and I find myself frustrated when I look at the layers being built on top of it.

I’m going to draw parallels to what are retrospectively considered bad designs, that in their hey-day were equally revered. Remember SOAP and the WS-\* mess? Today you’d be sexy if you called it a piece of garbage. Ten years ago you’d be a fool who just doesn’t understand the magical architectural beauty of SOAP. We went through a similar phase with CORBA. Around that time there was also UML. I have one word for Kubernetes to be weary of: OpenStack.

So what happened? Why is Kubernetes at risk of going down the same path? Because NOBODY is advocating for me — the app developer.

I’m an operations person, sure. But I am not the person who needs to demonstrate “cross-org influence”, and “differentiation”, and what not. I am the person who needs to demonstrate rapid, fast, easy, reliable, simple deployment, operation and execution of applications. My metric is how well my app performs. The platform is immaterial and irrelevant. Therein lies the Kubernetes dilemma.

### A brief history of “platforms to make platforms”

The real problem is there are NO OPINIONS at any layers. Two years ago, we heard how this was based on Borg. No matter that none of the YAML files made sense, but all you needed to hear was, “This is based on Google. So this is awesome. Forget that you haven’t actually run any real app on this. Forget that there’s no way to isolate pods from talking to each other. ‘Because Google.”

Then at KubeCon 2017, the narrative changed a bit. Kubernetes was never intended to be the platform. It was always intended to be a way to build platforms. Never heard that for two years before that. For two years I was told to write apps on what was allegedly never a platform in the first place.

Today I started investigating tools to deploy and manage applications on Kubernetes. I looked at Helm, Gitkube, Draft.sh, etc. You know what annoyed me the most? These tools were not platforms on top of Kube. Kube does a bit too much to ensure it can’t be completely abstracted out. But it does a bit too little to be a platform by itself.

Do you know why I hated SOAP? It isn’t because WS-\* was too complex or too painful, or that the tools never worked. SOAP broke a fundamental law for me — it wanted to be involved, but it never wanted responsibility. It wanted to always sit at the table, without actually solving anything concretely. SOAP was a “consultant”. I know of a dozen examples where everyone would break open a generated SOAP-proxy and typecast it so they could override an HTTP header for tracing, for instance. So… were we now okay actually using ALL the good things from HTTP? Not really. SOAP wanted a seat at the table. You couldn’t really cut it out and say — “You keep your WS-Security stuff to you. I’ll use HTTPS and be done with it.”

However if you then said, “Fine, do you just want to handle headers, metadata, etc? We’ll forget the transport. You handle versioning, endpoints, etc. etc.” and SOAP would back off… “Well… you see, things like these are complicated…. We need to really think this through… We cannot be hasty like this.”

SOAP wasn’t an API itself. It was a “toolset to create APIs.” Sound familiar?

OpenStack suffers from a similar problem. OpenStack isn’t a platform, it’s a way to build a platform. But then if you DO build a platform and try to abstract it out, it wants to be noticed. It wants a seat at the table.

#### Be a Dumb Pipe, or have Opinions

The real problem here is that all these technologies, and Kubernetes is dangerously close to getting there, is that they don’t want to be the Dumb Pipe. If every Kube cluster looked and acted the same, the vendor has very little differentiation opportunity. However, the differentiation can’t be too opinionated because it then cuts out other vendors from playing — it’s a huge gamble. You may win, or someone else may steal your lunch.

Kubernetes can’t really stay out of the way to the point that Helm is a complete platform that talks in higher constructs. No dealing with Pods and Controllers and ReplicaSets and Kubelets. That would make Helm or whatever it is the binding API that developers would stick to.

On the other hand, it Kubernetes can’t be the platform itself… for reasons I have no idea why. Why it is a platform to make other platforms, but I’m supposed to write apps directly for it none-the-less when that is convenient, is beyond me.

#### Cumulative cognitive load

So this is the crux of my complaint here. Every thing I touch with Kubernetes, adds more things I need to know, and takes NOTHING away. Not one thing!

To draw an analogy, let’s look at the TCP/IP stack. It is an example of exceptional layering. When I write an HTTP client, I don’t deal with window buffers, packet reordering, and what not. I assume it’s a stream. I don’t know packets. HTTP could very well be working on a circuit-switched line and I wouldn’t care. I care about URLs and Verbs. HTTP is far from perfect and it is supremely annoying. However HTTP has some strong opinions. Once you commit to it, you no longer get to play with packets. You could try but the other end is allowed to ignore you.

When I build an app, I really have a hard time wondering what Platform I’m writing to. I have to care about Kubelets, Pods, ReplicaSets, etc. etc. etc. But then if I want lifecycle management, I must additionally care about charts and configs and what not, without anything I no longer need to worry about. There is no, “You give up this abstraction, and in return you get an it-just-works-database. No more worrying about pods. It’s handled!”

The real key is, I’m an afterthought. How I deploy a web app is an afterthought. How I deploy or run a real database is an afterthought. I am a 2nd class citizen. I am asked to write to “the platform” when marketing, but when I complain, “it really isn’t the platform. this is a toolset to create platforms.”

#### I want to love you; help me love you

Here’s the call to the community. Forget platforms. Forget differentiation. Forget vendors. Forget marketing. Look out for the app developer. NOW!

SOAP was unbeatable until scrappy desperate people just used HTTP verbs to get stuff done. XML was unbeatable until desperate people just used JSON one day. SQL was unbeatable until desperate people just used simple Key-Value stores being backed into a corner. These changes came out of desperation to solve a problem.

I have a problem. What “platform” should I write to? Help me love you. Maybe I’m stupid. Maybe I don’t “get it”. Maybe complex long YAML files and reading a “concept dictionary” before I can write a goddamn web app is the right thing to do. I’m desperate, I’m scrappy and I have a problem.

If you want to avoid being the next SOAP, CORBA, UML and OpenStack, hear my plea. I jump on things that show benefits, or that make sense. This isn’t making sense to me right now. You need to tighten this up. You need to BE SOMETHING. Are you a platform? Then BE my platform. Are you NOT a platform? Then GET OUT OF MY WAY and NEVER tell me to write for you — instead refer me to the platform you want me to use. If you want a seat at the table, TAKE AWAY a worry I would otherwise have. Don’t be a consultant. Be a Product.
