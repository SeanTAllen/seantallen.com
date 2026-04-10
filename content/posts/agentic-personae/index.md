---
title: "Agentic Personae: Code and Cognition from Spotlights to Identity"
slug: "agentic-personae"
date: "2026-04-10T08:00:00-04:00"
draft: false
tags: ["LLM", "Ensemble Method"]
description: "How small attention focuses became full personas, and why the distinction matters for recurring LLM tasks."
summary: "An attention focus tells an agent where to look. A persona tells it how to think."
---

In [An Ensemble of Claudes]({{< ref "an-ensemble-of-claudes" >}}), I described a technique for getting better results from Claude Code: spawn multiple agents on the same task, give each one a slightly different attention focus, combine the results. An attention focus is a short directive. "Pay particular attention to security implications." "Focus on whether the test coverage exercises all boundary conditions." The agent covers everything regardless. The focus is a spotlight, not blinders. It shifts where the agent goes deeper.

Small differences in focus cascade through the reasoning chain, producing meaningfully different outputs. The combination outperforms any single attempt, the same way a random forest outperforms any single decision tree. I've been using the technique and it works.

But I kept typing the same spotlights. Code review after code review, I'd specify the same five or six attention focuses. Correctness, security, test coverage, API design. The same ones, every time. I was reconstructing knowledge I already had about what makes a good code review, invocation after invocation.

## Mojo

Matt Johnson is a friend and former colleague with a deep machine learning and computer vision background. He's one of a few people I talk with regularly about what's working and what isn't with LLMs. Matt, Sylvan, and I took the ideas from [Teaching Claude to Write Pony]({{< ref "teaching-claude-to-write-pony" >}}) and ran with them in our own directions, adapting and enhancing and changing for what works in each of our contexts.

When I published the ensemble post, Matt reviewed it and pointed me at what he'd been building for [microsoft/Trieste](https://github.com/microsoft/Trieste/tree/2874d0d7df123454255104dd839dcbb4f459166e/.github/skills). He hadn't been using small attention nudges. He'd built distinct personas, including an adversarial planner that works backward from failure and assumes every other planner has blind spots, a conservative planner that chooses the smallest diff and refuses speculative generality, a speed-focused planner that minimizes algorithmic complexity and optimizes cache locality. Each persona doesn't just look at different things. Each one thinks differently about the same things.

## The halfway point I'd already built

That reframing sent me back to something I'd already built without recognizing what it was. The [Antithesis research skill](https://github.com/antithesishq/antithesis-skills), a skill for analyzing codebases before testing with [Antithesis](https://antithesis.com), uses focuses like the ensemble method. You give each agent an area to concentrate on. But each focus comes with a paragraph explaining what it means. Not just "pay attention to concurrency properties" but a description of what concurrency properties look like in this kind of system, what patterns to watch for, what "going deep" means in that specific domain.

I hadn't thought of it as a step toward anything at the time. It was just how I'd built the skill. But looking at it after talking with Matt, I could see it for what it was: a halfway point. The focuses had more substance than bare spotlights, but they were fundamentally about *where* to look, just with better guidance about what to look for when you get there. They don't change how the agent approaches the problem. A focus on concurrency and a focus on data integrity both trace the code forward from implementation to consequences. They're looking at different things, but they're thinking the same way.

That's the distinction Matt's work made visible. His adversarial planner doesn't look at edge cases harder. It starts from "what would a bad outcome look like?" and works backward to find what produces it. That's a different mode of reasoning, not a different area of attention. I had a halfway point. Matt showed me what the destination looked like.

## What a persona is

I've built persona sets for code review, design, test planning, and documentation review. Code review is where the results are clearest, so I'll use it as the example. Take two of the code review personas, Correctness and Adversarial, and give them the same function to review. The Correctness reviewer traces logic forward. Given the inputs this code is designed to handle, does it handle them right? It follows each branch, each error path, each early return, verifying the code does what it claims. The Adversarial reviewer starts from the other end. It asks "what would a bad outcome look like?" and works backward to construct a concrete scenario that produces it. Specific inputs, specific sequences, specific expected-versus-actual outcomes.

Same function. Opposite directions. The Correctness reviewer might confirm every code path handles valid inputs correctly and miss that a specific hostile input causes a silent overflow. The Adversarial reviewer constructs that scenario because working backward from failure is its entire mode of thought. That's not a difference in coverage area. Both reviewers look at the whole function. The difference is cognitive, and it produces findings that the other approach structurally cannot reach through forward reasoning alone.

The other personas follow the same principle. The Tests reviewer applies counterfactual thinking: if the code under test were broken in the specific way this test targets, would the assertion fire? The API Design reviewer sketches consumer code first, writes the call sites, and evaluates whether the API is awkward to use before looking at the implementation. Forward reasoning, backward reasoning, counterfactual reasoning, consumer-perspective reasoning.

The actual persona definitions live in the [ponylang/llm-skills](https://github.com/ponylang/llm-skills) repo. To see what they looked like when this post was published, [here's a snapshot](https://github.com/ponylang/llm-skills/tree/f79fe5b73f7e9441ba3511f650091d815b73eb22).

## When each tool fits

The signal that a spotlight wants to become a persona is repetition. When you find yourself typing the same focuses into every invocation, you're reconstructing knowledge you already have. Building a persona means you stop reconstructing and start reusing.

But that signal only fires for recurring tasks. I do one-off work all the time: problems I haven't seen before, unfamiliar codebases, tasks I haven't done enough times to know which angles matter. The ad-hoc ensemble from [An Ensemble of Claudes]({{< ref "an-ensemble-of-claudes" >}}) is the right tool for that. Pick focuses that seem relevant, see what comes back, learn. The ensemble didn't get worse because personas exist. It got a companion for a different kind of problem.

Personas earn their keep when the task recurs, the reasoning approaches are known, and each perspective can do its work independently. Code review fits that description. So does test planning and documentation review. Each persona produces its findings in isolation, the synthesizer combines them, and the result covers more ground than any single pass. That independent-then-synthesize shape is where personas shine.

## Where personas struggle

That shape has limits. Software design is a different shape. Design is give-and-take between competing concerns. A conservative perspective and a performance perspective don't just each produce findings to be combined. They need to argue, make concessions, find the tradeoff that respects both. The persona architecture can't support that. Each persona talks to the orchestrator, not to each other. The synthesizer mediates between finished positions rather than facilitating a live negotiation.

When the principle-checker catches a violation that the consumer-first designer missed, the only path back is through the synthesizer's categorized "adjustment" in the next round. The consumer-first designer never hears the actual argument. It receives a verdict. Direct conversation would resolve in minutes what a round-trip through synthesis handles clumsily.

Claude Code has an experimental feature called agent teams that lets agents in a session communicate directly. That's the right mechanical fit. But if you let personas talk before each one has committed to an independent first pass, the loudest voice sets the frame and you lose the decorrelation that made multiple personas valuable in the first place. The independent first pass matters. It's what makes three perspectives genuinely different rather than three variations on whoever spoke first. Conversation is the negotiation layer that comes after. Getting that sequencing right is the open problem. I'll write about it when I have something worth reporting.