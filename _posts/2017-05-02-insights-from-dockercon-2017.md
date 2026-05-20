---
layout: post
title: "Insights from DockerCon 2017"
description: "The interest, adoption and, most importantly, innovation around the Docker platform continues to skyrocket. At DockerCon 2017 there was a…"
date: 2017-05-02 00:55:11 +0000
original_url: https://medium.com/@archisgore/insights-from-dockercon-2017-d908f84da521
---

The interest, adoption and, most importantly, innovation around the Docker platform continues to skyrocket. At DockerCon 2017 there was a vast number of innovators from around the globe and significant buzz around container-based approaches. With a little time behind us, I wanted to share three observations from the event.

**#1 Docker Has Leapt Beyond “Early Adopters” to Grab Mainstream Enterprise Interest**

It’s clear that containers and Docker are no longer just early-adopter technology. At the event we talked with many organizations who were primarily there to learn and ask questions. This class of more-conservative technology adopters had some hard questions and concerns based on deep enterprise experience at scale. They were very excited about Docker, and to see firsthand how it would work across their organization. Moreover, nearly every large organization we spoke to either already has a Docker story, or is working towards their first few production deployments. It’s a testament to the adoption of container-based approaches across all industries, ranging from technology giants to complex military applications.

**#2 Docker is Experiencing Rapid and Wide-Scale Innovation.**

There’s not just “interest” in Docker. There is rapid innovation within the Docker platform that is a testament to the wave of adoption around container-based architectures and the DevOps methodologies of rapid design and release.

For instance, Docker announced its [Moby](http://mobyproject.org/) project in the keynote, and it was exactly what I’d been hoping for. Using Hyperkit, InfraKit and LinuxKit, it has now become incredibly easy, reliable and predictable for us to manage our own infrastructure in-house, and also to ship prescriptive secure solutions to our customers. It’s been a week since we got back from DockerCon, and we already have prototypes on Moby that we’re integrating into our products over the next few months. We’re going to see the community rapidly adding to that library now that so much of Docker’s initial work is packaged into Moby as the heart of the platform.

There’s innovation happening with solutions providers (like us) and organizations (including Docker) that simply wasn’t possible before DevOps, containers and community combined to drive innovation. Intel demonstrated replacing Docker’s default “runc” container runtime with its own, which isolates container kernels; that nullifies a key question we always get about security: what happens if the kernel was zero-day’d?

What continues to impress me year over year is how deeply modern Docker is in its architecture and design. Docker is obsessively “unix-like” in its thinking: it continues to build small, independently usable, focused, targeted tools to solve specific problems, and then compose those tools together to provide a “platform.” In Docker’s own words, it is “batteries included, but swappable.” I am a big proponent of this design philosophy, and that makes Docker very appealing to me.

In addition, the secure-by-design system it is developing is very appealing. That includes using Notary for trust, which then informs Moby to build out trusted, predictable, reliable components; using InfraKit to deploy and manage instances; and Swarm to handle in-cluster secure data handling. All this helps developers and security-solution providers such as Polyverse easily amplify and expand the baseline security in the Docker platform.

At Polyverse, we leverage and build upon these existing Docker security and management initiatives to add the highest level of security for now-critical container applications, with very little effort from developers. We take advantage of these features in our Moving Target Defense (MTD) solutions by rapidly cycling containers, providing scrambled binaries, and so on. Most of our MTD features are included in our Docker-certified [Microservice Firewall](https://polyverse.io/assets/docs/Polyverse_Docker_Press_Release_030217.pdf).

**#3 Many Orchestration Choices Remain, and the Market Will Decide Quickly**

The still unresolved problem in the container space is that of orchestration. There are many models of how containers should be managed at scale: Docker’s own Swarm, Google’s open-source Kubernetes project, Amazon’s ECS, Mesos and Mesosphere, Rancher’s Cattle, and CoreOS. Some of these are moving towards consolidating their APIs — for instance Rancher supports multiple orchestrators managed by their base orchestrator. CoreOS is supporting Kubernetes.

The benefits and drawbacks of each is a discussion (or blog) for another time. I personally find something to love in each of them, but each also has its pain points. This is the challenge in any project. You always want everything all at once, but you have to pick one thing first. Prioritization always leaves some people ecstatic and others unhappy, in the short-term at least.

Some of these projects started with developers in mind, while leaving production deployment stories “yet to be defined.” Others started with production operators at scale in mind, and are still catching up in terms of providing an easy-to-use developer experience.

In the end, they will all likely have the same set of operational features, and will expand to what developers demand. For what it’s worth, I personally admire various thoughts, concepts and philosophies being tested through different scenarios as a means to discover what’s best.

If we missed you at DockerCon, don’t hesitate to drop me a line at [archis@polyverse.com](mailto:archis@polyverse.com).
