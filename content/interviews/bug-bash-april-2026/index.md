---
title: "Why Simple Workloads Find the Hardest Bugs"
tags: ["Testing", "Antithesis"]
slug: "why-simple-workloads-find-the-hardest-bugs"
draft: false
date: "2026-04-08T07:00:00-05:00"
imagePosition: "center 66%"
description: "Marco Primi and I sit down with David Wynn to discuss workloads and testing distributed systems."
summary: "Marco Primi and I join David Wynn on Bug Bash to talk about workloads — what they are, why they matter for testing distributed systems, and how even a simple `while true` loop can expose bugs your unit tests miss."
---
## Interview overview

You hit 100% test coverage, deploy to production, and the system still falls over. An old story, maybe aside from the 100% coverage percent part.

In this episode, David sits down with Marco Primi and Sean Allen from the Antithesis Red Team to talk about a likely missing piece in your testing strategy: workloads.

They break down exactly what workloads are, why they are so helpful for testing distributed systems, and the biggest pitfalls engineers hit when trying them out. They talk about why you have to let go of deterministic, "happy path" testing, and how even a workload as "dumb" as a basic `while true` loop can expose silent data loss that your unit tests would never catch.

## Links

* [YouTube version](https://www.youtube.com/watch?v=N-EiBO3YA5E)

## References

* [Hegel](https://hegel.dev/)
* [Hypothesis](https://hypothesis.readthedocs.io/en/latest/)
* [Jepsen](https://github.com/jepsen-io/jepsen)
* [Property-based testing](https://antithesis.com/docs/resources/property_based_testing/)
