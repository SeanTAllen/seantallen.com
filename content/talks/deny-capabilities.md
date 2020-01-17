---
title: "On Deny Capabilities for Safe, Fast Actors"
slug: "deny-capabilities"
date: 2020-01-01T00:00:00-04:00
draft: false
tags: [Pony, Scaling]
description: "How Pony combines Deny Capabilities with an actor-based model to provide data-race freedom and fearless concurrency."
---
## Abstract

["Deny Capabilities for Safe, Fast Actors (2015)"](https://www.ponylang.io/media/papers/fast-cheap.pdf) by Sylvan Clebsch et al. lays out the core novel idea behind the Pony programming language: deny capabilities. Deny capabilities take object capabilities, turn them on their head and then apply them to variable aliases.

In this talk, Sean T. Allen (member of the Pony core team) explains Deny Capabilities and how Pony combines them with an actor-based model -- to provide a programming environment that allows for data-race freedom and fearless concurrency.

## Versions of this talk

* Papers We Love NYC January 8, 2020 - [Slides](https://gitpitch.com/SeanTAllen/on-deny-capabilities/) | [Video](https://www.youtube.com/watch?v=PkOHZEJeJtQ)

## Links

* [Deny Capabilities for Safe, Fast Actors](https://www.ponylang.io/media/papers/fast-cheap.pdf)
* [Pony](https://www.ponylang.io/)
* [Pony Twitter](https://twitter.com/ponylang)
* [My Twitter](https://twitter.com/seantallen)
* [My Personal Website](https://www.seantallen.com/)

## References

* [Coordination Avoidance in Database Systems](http://www.vldb.org/pvldb/vol8/p185-bailis.pdf)
* [Silence is Golden: Coordination-Avoiding Systems Design](https://www.youtube.com/watch?v=EYJnWttrC9k)
* [Race Condition vs. Data Race](https://blog.regehr.org/archives/490)
* [RWMutex scales poorly with CPU count](https://github.com/golang/go/issues/17973)
* [The wide world of almost-actors: comparing Pony to BEAM languages](https://www.youtube.com/watch?v=_0m0_qtfzLs)
