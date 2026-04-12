---
title: "GC Made Fast"
slug: "gc-made-fast"
date: 2026-04-12T00:00:00-04:00
draft: false
tags: [Pony, Scaling, Garbage Collection]
description: "How Pony's actor-based design turns garbage collection from a coordination bottleneck into a local, fast operation."
summary: "Most engineers think GC is slow. They're wrong. Coordination is slow. Pony's per-actor heaps and message-passing model eliminate the coordination that makes traditional garbage collectors a performance problem."
---
## Abstract

Most engineers think garbage collection is slow. They're not wrong about their experience — stop-the-world collectors really do destroy your tail latencies. But "GC is slow" confuses the symptom with the cause. The real villain is coordination. Every stop-the-world pause, every atomic reference count, every lock-free reclamation scheme is coordination over memory, and coordination is what kills performance.

Pony takes a different path. Actors own their own heaps. Each actor collects independently, using only local knowledge. No stop-the-world. Ever. When actors need to share information about cross-actor references, they gossip — piggybacking on the application messages they're already sending. They share information but never wait for agreement to act.

This isn't magic and it isn't free. But the costs are local, not coordination costs. And that changes everything about what "fast GC" can mean.

## Versions of this talk

* NYC Systems - [Slides](/presentations/gc-made-fast/nyc-systems/)

## Links

* [Pony](https://www.ponylang.io/)

## References

* [Fully Concurrent Garbage Collection of Actors on Many-Core Machines](https://www.ponylang.io/media/papers/opsla237-clebsch.pdf)
* [Life Beyond Distributed Transactions - 2007 version](http://www-db.cs.wisc.edu/cidr/cidr2007/papers/cidr07p15.pdf)
* [Life Beyond Distributed Transactions - 2016 version](https://queue.acm.org/detail.cfm?id=3025012)
* [ORCA: GC and Type System Co-Design for Actor Languages](https://www.ponylang.io/media/papers/orca_gc_and_type_system_co-design_for_actor_languages.pdf)
* [Ownership and Reference Counting based Garbage Collection in the Actor World](https://www.ponylang.io/media/papers/OGC.pdf)
* [Pony: Co-designing a Type System and a Runtime](https://www.ponylang.io/media/papers/codesigning.pdf)
* [Silence is Golden: Coordination-Avoiding Systems Design](https://www.youtube.com/watch?v=EYJnWttrC9k)
* [Trash Day: Coordinating Garbage Collection in Distributed Systems](https://www.usenix.org/system/files/conference/hotos15/hotos15-paper-maas.pdf)
