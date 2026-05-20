---
layout: post
title: "Write go-plugin’s in Rust"
description: "Use the grr-plugin crate to write plugins into Golang applications from Rust."
date: 2022-02-04 22:00:13 +0000
original_url: https://medium.com/@archisgore/write-go-plugins-in-rust-5e7afcabde6d
---

Use the [grr-plugin](https://crates.io/crates/grr-plugin) crate to write plugins into Golang applications from Rust.

Golang is everywhere and famously lacks the ability to load modules. I know a number of tools utilize the [go-plugin implementation by HashiCorp](https://github.com/hashicorp/go-plugin) to consume gRPC-based plugins.

In the readme this implementation appears trivial — you open a grpc server and print a handshake string to standard out that tells the client (plugin consumer) how to connect to you. This illusion of triviality is further reinforced by the [python example](https://github.com/hashicorp/go-plugin/tree/master/examples/grpc/plugin-python) which demonstrates a plugin written in a non-Go language.

Underneath though, the go-plugin spec does a bunch of useful stuff that is fairly complex. Allowing apps to negotiate multiple side-channels of communication using either more gRPC or JSON-HTTP 2.0 spec.

All of this is handled in grr-plugin.

There are no unit tests or tests of any kind. I’ll add them once I get my canonical use-case working, which is a [Rust-based Custom VM integrated into the Avalanche blockchain](https://github.com/archisgore/landslide). More on this project later.
