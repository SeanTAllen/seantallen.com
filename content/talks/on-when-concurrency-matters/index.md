---
title: "On When Concurrency Matters: Behaviour-Oriented Concurrency"
slug: "on-when-concurrency-matters"
date: 2026-05-08T00:00:00-04:00
draft: false
tags: [Pony, Concurrency, Programming Languages]
description: "How Behaviour-Oriented Concurrency keeps the actor model's strengths and replaces its biggest weakness with a single keyword."
summary: "The actor model gives you safety and scale, but operations across multiple actors turn into painful protocols. Behaviour-Oriented Concurrency keeps the strengths and replaces the protocols with a single keyword."
---
## Abstract

The actor model is one of the great workhorses of concurrent programming. It gives you data race freedom by construction. It scales horizontally as far as the hardware will let it. It lets you reason locally, one actor at a time, one message at a time. Erlang has been doing it for decades. Akka brought it to the JVM. Pony built a whole language around it.

But the actor model has an Achilles heel. Any operation that needs to atomically span multiple actors becomes a protocol. A dance of messages. State machines arguing with state machines across the wire. Easy to get wrong, painful to extend, and so noisy that the actual business logic gets buried under the choreography.

Behaviour-Oriented Concurrency is a paradigm that keeps what the actor model gets right and rethinks what it gets wrong. Instead of state-bound actors trading messages, you have behaviours: units of concurrent work that atomically acquire whatever resources they need to run. The runtime guarantees the acquisition is atomic, the execution is exclusive, and the ordering is consistent. No protocol. No state machines arguing across the wire. Just `when (x, y, z) { ... }`.

This talk works through the BoC paper from OOPSLA 2023, by Cheeseman, Parkinson, Clebsch, and friends. We start with what the actor model gets right and why so many people built systems around it. We walk to where it falls down. Then we get to how BoC fixes the gap without giving up what makes the actor model worth using.

## Versions of this talk

* Papers We Love San Francisco - [Slides](/presentations/on-when-concurrency-matters/pwl-sf/)

## Links

* [bocpy - a Python implementation of BoC](https://microsoft.github.io/bocpy/)
* [Verona - the language BoC is being implemented in](https://github.com/microsoft/verona)
* [Pony](https://www.ponylang.io/)

## References

* [When Concurrency Matters: Behaviour-Oriented Concurrency](/papers/external/when-concurrency-matters.pdf)
* [Reference Capabilities for Flexible Memory Management](/papers/external/reference-capabilities.pdf)
* [Deny Capabilities for Safe, Fast Actors](https://www.ponylang.io/media/papers/fast-cheap.pdf)
* [Pony: Co-designing a Type System and a Runtime](https://www.ponylang.io/media/papers/codesigning.pdf)
