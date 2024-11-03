---
title: "Pony: High-Performance, Memory-Safe Actors with Sean Allen"
tags: ["Pony"]
slug: "high-performance-memory-safe-actors"
draft: false
date: "2024-11-03T07:00:00-05:00"
description: "Kris Jenkins and I discuss Pony and some of its unique features."
---
## Interview overview

Pony is a language born out of what should be a simple need - actor-style programming with C performance. On the face of it, that shouldn’t be too hard to do. Writing an actor framework isn’t trivial, but it’s well-trodden ground. The hard part is balancing performance and memory management. When your actors start passing hundreds of thousands of complex messages around, either you need some complex rules about who owns and frees which piece of memory, or you just copy every piece of data and kill your performance. Pony’s solution is a third way - a novel approach to memory management called reference capabilities.

In this week’s Developer Voices, Sean Allen joins us from the Pony team to explain what reference capabilities are, how Pony uses them in its high-performance actor framework, and how they implement a garbage collector without stop-the-world pauses. The result is a language for performant actors, and a set of ideas bigger than the language itself.

## Links

* [YouTube version](https://www.youtube.com/watch?v=u9da3UzEhEI)
* [Other Listening Options](https://pod.link/developer-voices/episode/89440d0d3be17217212d113cf6a31400)

## References

* [Pony website](https://www.ponylang.io/)
* [Pony Tutorial](https://tutorial.ponylang.io/)
* [Pony Playground](https://playground.ponylang.io/)
* [Azul Garbage Collector](https://www.azul.com/products/components/pgc/)
* [Shenandoah Garbage Collector](https://wiki.openjdk.org/display/shenandoah/Main)
* [A String of Ponies (Distributed Actors Paper)](https://www.ponylang.io/media/papers/a_string_of_ponies.pdf)
* [Garbage Collection with Pony-ORCA](https://tutorial.ponylang.io/appendices/garbage-collection.html)
* [Orca: GC and Type System Co-Design for Actor Languages](https://www.ponylang.io/media/papers/orca_gc_and_type_system_co-design_for_actor_languages.pdf)
* [Ownership and Reference Counting based Garbage Collection in the Actor World](https://www.ponylang.io/media/papers/OGC.pdf)
* [Fully Concurrent Garbage Collection of Actors on Many-Core Machines](https://www.ponylang.io/media/papers/opsla237-clebsch.pdf)
