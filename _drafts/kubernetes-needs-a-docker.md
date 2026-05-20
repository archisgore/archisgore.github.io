---
layout: post
title: "Kubernetes Needs a Docker"
description: "Docker was never about Containers, and we’re all the worse for it"
date: 2026-05-20 17:47:51 +0000
---

Docker was never about Containers, and we’re all the worse for it

### It began with Google’s invention of the internet…

Google introduced the world to the new concept of “Declarative Programming” in 2014, along with “Sidecars” and for the first time, a way to “automatically restart a process when it crashes”! I don’t miss the days leading up to 2014, the dark ages of Ops where we’d `ssh` and `ps aux` our VMs once every hour and run commands to restart missing processes by hand.

Among the many things Google invented, they also invented these things in Linux that allowed for processes to be resource-partitioned and for their environment to look like… “root”.

### Along came Docker

If you watch the one video that changed it all, Docker changed the way
