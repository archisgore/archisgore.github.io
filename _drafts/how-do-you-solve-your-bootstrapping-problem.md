---
layout: post
title: "How do you solve your bootstrapping problem?"
description: "When everything is obvious, yet nothing is."
date: 2026-05-20 17:47:51 +0000
---

For about ten years now, there is a consistent problem that bugs me personally.

Anytime I read or hear about this grand new world-changing thing that makes everything easy, simple, obvious. So I try to use it…. but… wait, I must first learn an aboriginal rain dance, sacrifice 2 heads of my finest calves, replace my shell, install a specific version of python, with 3 specific native modules, maybe change my entire OS, learn a new TOML, YAML, or whatever the latest fad text language is, etc.

Once I do that, however, whatever this latest and greatest tech is, it’s going to be soooooo trivial to set up… well, hold on there. Except that now I need some SSL certs, a root CA, maybe self-signed, maybe not. Until [OMGWTFSSL](https://hub.docker.com/r/paulczar/omgwtfssl/~/dockerfile/) came along, I always had to ask the internet for how to generate certs. Every single time!

I wonder though, at that point, is the greatness of the tech that it secretly got me to take an IT certification course, or was the tech really that simple?

For me, building Linux from scratch is simple. Then again, my company builds Linux a few hundred times a day. If you asked me to show you, I would build you any package at all, within minutes, given the compute power.

And yet, building Linux packages is not simple. It is why the most popular distributions don’t give you source tarballs.
