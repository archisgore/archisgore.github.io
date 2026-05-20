---
layout: post
title: "Hi Doug! Yes indeed — very early Mesh alum."
description: "Glad to hear you work on the CDK. It’s a wonderful product."
date: 2019-04-09 14:04:01 +0000
original_url: https://medium.com/@archisgore/hi-doug-yes-indeed-very-early-mesh-alum-85b5d9d6067b
---

Hi Doug! Yes indeed — very early Mesh alum.

Glad to hear you work on the CDK. It’s a wonderful product.

About your question on whether Prolog or not — slightly. But you could say F# or Lisp for all I care. Here’s the difference between the CDK and what I’m proposing (and why I’m criticizing Kubernetes and Google at large for coopting Declarative to mean “Static Configuration”).

The CDK is CodeGen (yuck! yuck! yuck!). Which makes no sense, because underneath the Declarative CloudFormation is imperative code. Same for Kubernetes Operators — it’s ALL imperative code, that is *configured* Declaratively.

Now that configuration became too large, so we wrote imperative code on top? Secondly the underlying declarative “layer” doesn’t understand migration semantics. And there’s no way to teach it. No way to declare it. So you have to know magically if underneath the CloudFormation template how the behavior is coded.

What I’m proposing is not “Generate Verbose Static Configuration for underlying Imperative Systems using wrapper Imperative Code.” I’m more than happy to write out scenarios and situations why this is useful. Think of a migration from InfraV1 to InfraV2. What do you do? FOUR language levels:

AWS SDK <- Cloud Formation YAML <- AWS CDK <- Upper-level Scripts

You use CDK to create the new CF stack which uses the SDK to manifest the stack, and then your outer-controller/thingy moves data across, waits for bootstrap, let’s load balancers warm up, let DNS resolve to them, etc. etc. then tear down the stack.

It’s why we directly do this:

AWS SDK <- Controller Scripts

And use neither Pulumi nor the CDK. The detached-approach leads to major problems — currently we have stuck EIPs launched by CF, that AWS support won’t help with, we can’t diagnose where they came from, where they’re bound to, and it’s costing resources. We deleted the CF stack, but there’s a conflict. Why? Because Code-Generation is a TERRIBLE IDEA!! The causality is lost. The trail is cold.

Think if a Compiler — if C or Java compiled to Machine code, but then the debugger only gave you machine code, you’d go insane. Now you have to actually remember what each line of C translates do. Instead by moving up an abstraction, C has to take on a larger burden. It provides a debugger which debugs at the “C Machine” — statement by statement, and variable by variable.

What the CDK does is, it gives me C when selling to me, but then makes me debug in Assembly when it comes to supporting it.

Prolog is a language I like, but the problem I face could be solved by any real language, but a real language to talk TO AWS, not to generate the thing that is then parsed by another thing that talks to AWS.
