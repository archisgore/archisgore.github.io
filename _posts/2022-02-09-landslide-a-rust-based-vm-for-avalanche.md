---
layout: post
title: "Landslide: A Rust-based VM for Avalanche"
description: "https://github.com/archisgore/landslide"
date: 2022-02-09 03:01:47 +0000
original_url: https://medium.com/@archisgore/landslide-a-rust-based-vm-for-avalanche-35f5a49da6a5
---

### **Why?**

How I got interested in blockchain and crypto is a longer story, better told in person.

What we did discover, after speaking to lots and lots of people is there are a whole lot of legitimate use-cases for using a distributed immutable ledger, that have nothing to do with currency.

None of these are revolutionary, but each of them could benefit from evolution. A few that we are convinced of:

1. Consider data used for published academic research. If a publisher reaches a particular conclusion based on the data, a published “smart contract” can validate the repeatability (though not the reproducibility) allowing anyone in the future to easily validate the conclusions drawn.
2. Consider training an AI model to be non-racist and non-misogynist. Publishing the training set on a distributed system and publishing the training methodology to be cross-verified may provide better visibility for consumers into many of the systems we use, and provide a reduction in liability for those publishing those systems (think self-driving cars, facial recognition, etc.)
3. P2P games where you directly connect to each other, and the “game play” is directly coded as smart contracts. You spin up “a chain” between 5 players, and play the game. The chain overall validates all moves are valid.

Aside from these, there are a number of opportunities to improve on the current state of the art:

1. You may want locked-down audited, verified, signed, certified blockchains smart contracts in a safe language.
2. You may want to access GPUs, inference/neural engines, lots of storage or memory, and more, in smart contracts.
3. Smart Contracts should be writable in a whole host of easily-pluggable scripting languages.

### How?

Let me introduce you to Landslide, a Timestamp VM implementation for Avalanche, 100% in Rust. I expect it to become the basis for many other VMs that can do incredible things.

I also expect this to open up a number of other applications such as auditable and immutable smart contracts, smart contracts that can use native system resources, VMs that can plug in a host of other scripted and unscripted languages, entire game engines coded as a VM, and so much more.
