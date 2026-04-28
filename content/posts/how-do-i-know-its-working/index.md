---
title: "How Do I Know It's Working?"
slug: "how-do-i-know-its-working"
date: "2026-04-27T08:00:00-04:00"
draft: false
tags: ["Software Design", "Testing"]
description: "Why I test, even though I don't value tests."
summary: "I test because I want to know what I've built is working. That's very different than valuing testing."
---

No one cares about testing. That's going to hurt the feelings of the people who do care about testing, but, I'm going to stand by the statement. That might sound odd coming from someone who has a reputation for talking about testing, but — I don't value tests. Tests are a means to an end. The means are unimportant. The end is all that is important.

I don't sit down to write code. I sit down to build something. The code is just how I get there. I start by designing. The design is guided by one question: how will I know it's working?

## The exercise

A few years back, I was VP of Engineering at a company building a high-performance distributed stream processor. I wrote the coding exercise we used in engineering interviews. It looked simple: design an in-memory key/value store. Put. Get. Delete. Add optional expiration. Make it thread-safe. Hit 95th-percentile reads under a millisecond at ten million keys.

Most of the exercise was straightforward. The performance targets were easy. You'd have to try hard not to hit them. The hard part was a single line buried in the requirements: verify that the functional requirements were met and worked.

The straightforward designs — a hashmap behind a mutex, a sharded map, a concurrent skip list — all work. They hit the performance targets. None of them give you a way to verify what you built actually holds up under load, under concurrency, against failures the spec didn't spell out. And what does "correct" even mean when two writes hit the same key at the same time? The spec didn't say. A rare few candidates started from "how will I know it's working?" and built outward. They went to the top of the pile.

Most didn't. That was fine. I could usually find bugs in their code, and what mattered was their reaction. Show them a bug. Did finding it make them confident the rest had been found, or did it make them question the whole thing? Walk them through where the design didn't let them know whether it worked. Were they ready to rethink, or were they done? The candidates I cut were the ones who, after I showed them bugs, were still sure their approach was fine and there wouldn't be more. The candidates I wanted to hire knew what they didn't know and wanted to find out. Perfection wasn't required. Reaching for it was.

Leslie Lamport has a line about thinking I keep coming back to: "If you're thinking without writing, you only think you're thinking." I'd extend it. You aren't really designing until you've started writing down how you'll know it works.

## Make wrong impossible

The strongest move is to make the failure impossible. The compiler answers "is it working?" for you, every time you build.

This is why I love OCaml. Why I love F#. Why I love Pony. They turn questions about my code into questions the compiler answers.

In Pony specifically, data races on mutable shared state aren't just unlikely. They aren't expressible. The compiler refuses to compile code that could race. Whole categories of concurrency bugs that I'd write tests for in another language? I don't write those tests. I can't write the bugs.

What Pony does to data races is one form of a broader principle: make illegal states unrepresentable. When I can encode a constraint in the type system, I never have to write a test for it. The compiler is the test I didn't have to write.

Years ago, I wrote a [post](/posts/learning-from-ada/) about choosing C over Ada as a young programmer. The reason I picked C is that Ada's compiler frustrated me. I couldn't get my code to compile. What I didn't understand for a long time is that Ada's compiler being annoying was Ada doing its job. It was telling me my code was wrong, and I was treating that as friction instead of evidence.

## Means and ends

Types are one of the tools I lean on. There are others. I'm not going to run them all down here. All of them are means.

The end is a system that works. A system you can change without dread. A system you can reason about. One that ships, and keeps shipping.

That's what I'm after. Not tests. Not even types. The working system.

A working system lives or dies in production. That's where the design proves itself, or doesn't.

Back in 2016, I gave a talk called "[How Did I Get Here?](https://www.seantallen.com/talks/how-did-i-get-here/)" about building confidence in a distributed stream processor. I answered with fault injection, message tracing, and auditing. The system I was working on has changed. The tools I'd reach for have changed. The question — how do I know it's working? — hasn't.
