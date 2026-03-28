---
title: "An Ensemble of Claudes"
slug: "an-ensemble-of-claudes"
date: "2026-03-28T08:00:00-04:00"
draft: false
tags: ["LLM", "Ensemble Method"]
description: "How an accidental observation about prompt sensitivity led to a structured ensemble method for Claude Code."
---

I wrote about [teaching Claude to write Pony]({{< ref "teaching-claude-to-write-pony" >}}) a few weeks ago. The biggest breakthrough in that process was the reviewer loop: spawn a fresh Claude instance with no shared context, hand it a set of engineering principles and the code, let it review. Iterate until the review comes back clean. For anyone who hasn't read that post, the short version is that having multiple fresh perspectives check work against a shared set of principles dramatically improved both quality and trust.

The reviewer loop had multiple fresh Claudes checking one Claude's work. This post is about what happened when I started having multiple Claudes do the work itself, each from a different angle, and combining the results.

## The discovery

I work at [Antithesis](https://antithesis.com). Antithesis is a testing platform that runs your software deterministically, exploring many parallel executions with different orderings, failures, and timings to find bugs that only show up under specific conditions. I [wrote about it]({{< ref "peter-alvaro-and-me" >}}) a few weeks back.

Before you can test a system with Antithesis, you need to understand it. Not in the casual "I read the README" sense. You need to understand the architecture, the data flows, where state lives, how components talk to each other, how they fail. You need to figure out which properties should hold under fault injection and which parts of the system are most likely to break in ways that matter. That requires real depth in both the codebase and the problem domain. A testing platform that explores failure scenarios is only as good as your understanding of what the system is supposed to do in the first place.

We have a [set of skills for LLMs](https://github.com/antithesishq/antithesis-skills) that help users get their systems set up for testing with Antithesis. One of those is a research skill. You point it at a codebase and it analyzes the system: maps the architecture, identifies failure-prone areas, and discovers testable properties. The output is a substantial research report. Getting a good one requires the LLM to go deep on a codebase it has never seen before and produce analysis that's useful to someone who is going to use it as a guide when making testing decisions.

I was working on improving the results the research skill was producing. Tweaking the prompt inputs. Small changes. Adjusting which aspects to emphasize, nudging how the attention was directed. And the outputs were often wildly different. Not wrong. Just differently focused. The broad strokes were usually the same across runs, maybe 80% overlap. But the depth varied a lot. One run would go deep on concurrency properties while staying shallow on data integrity. Another would thoroughly cover failure recovery paths but give protocol contracts only surface treatment. Each output was competent. Each left different areas underexplored.

I decided to try combining them. I directed Claude to read the outputs from multiple runs and synthesize them into a single report. The result was better than any of the individual runs. Not just a little better. The deep concurrency analysis from one run filled in the gaps where another run had gone shallow. The thorough failure recovery coverage from a second run covered what the first had skimmed. The combined report covered the problem space in a way none of the individual runs did.

So I kept going. Generated more reports with different emphasis, added them to the pile, and had Claude synthesize the whole batch. Each time I added another report, the synthesis got better. That's when I started thinking this wasn't a fluke. But I wanted to refine the technique further, and research reports on large codebases weren't the place to do it. They're long, and evaluating whether a report is actually good requires deep expertise in the target system. I needed a domain where I could recognize quality quickly.

## The machine learning connection

What I was doing belongs to a family of techniques in machine learning: ensemble methods.

Combining multiple models often outperforms any single model, even a really good one. The reason is about what each model misses.

Take a [decision tree](https://en.wikipedia.org/wiki/Decision_tree_learning). A single model that makes a chain of decisions based on features of the input. For any given input, it follows one path to one answer, and whatever's down the roads not taken, it doesn't see. A single Claude agent works the same way. Ask it to "analyze this system" and it picks up a thread and follows it. What it finds shapes where it looks next. It goes deep on whatever it happened to start with and shallow on everything else.

A [random forest](https://en.wikipedia.org/wiki/Random_forest) runs many decision trees, but each one only sees a random subset of the features. Each tree is individually weaker because it literally can't see the whole picture. But each tree sees *something different*. Aggregate their predictions and you get something better than the best individual tree. The trick isn't in any one tree. It's in the diversity. Each tree makes different mistakes, and the mistakes cancel when you combine them.

[Boosting](https://en.wikipedia.org/wiki/Boosting_(machine_learning)) takes a different tack. Train models sequentially, each one focusing on what the previous ones got wrong. I wasn't doing that, but it's the same core idea from yet another angle: you can get better results from multiple decorrelated attempts than from one really good attempt.

What I was doing wasn't any of these exactly. But it's in the same family. Same idea: decorrelated approaches to the same problem, combined, outperform a single approach. In a random forest, the decorrelation comes from random feature subsets. In my approach, it comes from attention focuses, small directives that shift where the agent concentrates. Same principle, different mechanism.

## Pony as the lab

As I described in [Teaching Claude to Write Pony]({{< ref "teaching-claude-to-write-pony" >}}), I've been teaching Claude Code to write [Pony](https://www.ponylang.io/), a programming language I've worked on for years. I'd built up a set of engineering principles in Claude's instructions, and I had a reviewer loop working: fresh Claude instances checking work against those principles, iterating until the review comes back clean. I was getting good results on real projects.

Pony was the right place to explore the combining idea further. I've been writing Pony for a long time. I can look at a code review or a design plan and know within minutes whether it's good. The artifacts are smaller and more self-contained than a research report on a large codebase. When a code review is thorough, I can tell. When a design plan misses something, I can tell. That kind of fast, confident evaluation is exactly what you need when you're trying to refine a technique. So Pony became the lab. I'd seen that combining differently-focused outputs produced better results. Now I wanted to test that idea deliberately.

I already had the reviewer loop from the earlier Pony work. In that workflow, one Claude does the work, then a fresh Claude with no shared context reviews it against the engineering principles. If the reviewer finds issues, the worker fixes them and another fresh reviewer checks again. Repeat until the review comes back clean. It's a quality gate.

The idea of having multiple agents do the same work from different angles and combining the results was structurally similar to the reviewer loop: spawn sub-agents, get back outputs, combine. But it served a different purpose. The reviewer loop checks work after it's done. The ensemble, as I started calling it, produces the work itself.

The mechanism: each sub-agent gets the same task but a different attention focus. A short directive that shifts where the agent concentrates. Not blinders. The agent still covers everything. It's a spotlight, not a filter. "Pay particular attention to security implications." "Focus on whether the test coverage exercises all boundary conditions." "Evaluate the API ergonomics from a first-time user's perspective."

I'd prompt something like: "Review this PR using an ensemble method. Foci: correctness, completeness, security, performance, test coverage." Five agents. Same PR. Same principles. Same CLAUDE.md. Each comes back with different results. The test coverage agent catches gaps the others missed, the security agent spots an input validation issue no one else flagged, and the completeness agent finds a missing edge case in a state machine. Three things I would have caught eventually, but not in a single pass.

For a design review of a new API in a Pony library, the focuses might be different: type safety, API ergonomics for new users, consistency with existing patterns, error handling completeness, boundary condition coverage in the test plan. The type safety agent catches a capabilities issue the others gloss over. The ergonomics agent flags a confusing constructor, and the consistency agent notices a naming convention break that would have bugged me in code review.

Each agent goes deeper on its focus than any single "general" agent would. A single agent spreading attention across everything is the decision tree: one path, one answer. The ensemble is the forest.

I tell Claude when to use this. It's not automatic. Claude loads the ensemble skill, I provide the attention focuses, and the agents spin up in parallel. Each one starts with fresh context, no shared conversation history, just like the reviewers from "Teaching Claude to Write Pony." That fresh start is part of why small differences in attention focus cascade into meaningfully different outputs.

## The synthesis agent

The ensemble produces raw material from multiple agents. Somebody has to combine it. That somebody is the synthesizer, another agent whose job is to take the outputs from all the ensemble agents and integrate them into a single result.

Sometimes one agent just nails it. When one output is clearly superior and the others are noise, the right move is to use that output and say so. Blending for the sake of inclusion is a failure mode. The ensemble exists to find the best result, not to be democratic about it.

That doesn't mean defaulting to "pick the winner." Most of the time, no single agent dominates. The synthesizer's real work happens in those cases. It finds where agents agree, and those agreements become the foundation. It resolves divergences by evaluating on merits. If the security-focused agent and the correctness-focused agent disagree about an approach, the question is which approach is actually better, not which focus area gets priority.

The intersections are where it pays off. One agent rejects an alternative that another agent uses successfully. The synthesizer notices and asks why. Sometimes that's where you find solutions none of the agents would have reached alone. The synthesizer also cross-references uncertainties against other agents' confident conclusions. When one agent says "I'm not sure about X" and another says "X is definitely Y, here's my evidence," that uncertainty is resolved. When all agents share the same uncertainty, that's a genuinely hard problem worth flagging for me.

The synthesis step is where the ensemble produces something none of the individual agents could have. The synthesis isn't the union of all findings. It surfaces things no individual agent found, things that only emerge from the combination.

## What I learned making it work

I spent about five days refining the ensemble on Pony work. Different numbers of agents, different kinds of focuses, trying to learn what produces useful diversity and what's just noise.

The biggest lesson was about what kind of focuses actually produce decorrelated outputs. I tried all sorts of things. Small perturbations like "review for correctness" and "review for security" look tiny compared to big persona-based prompts like "review as a Pony expert" or "review as a security consultant." But the small focuses produced results that were just as good. You don't need to ask the agent to pretend to be a different expert. The agents are the same model with the same knowledge. A small nudge in attention is enough because small differences cascade through the reasoning chain. Like a random forest: the perturbation is small, but the downstream effects are large.

Output format turned out to matter more than I expected. Early on, combining the outputs was like panning for gold: I could tell the good stuff was in there, but finding it was work. So I gave each agent a structured format. Flag your key decisions and what alternatives you considered. Flag your uncertainties. Flag your assumptions. Once the synthesis agent had that structure, it could cross-reference rather than guess. One agent's uncertainty might be another agent's confident conclusion. One agent's assumption might be something another agent explicitly verified. The structure turned synthesis from an art into something closer to a procedure.

For the Pony design and review work, three to five agents turned out to be the sweet spot. More than that and I was paying for repetition without getting meaningful diversity. Other domains with larger problem spaces might need more focuses.

And not every task needs an ensemble. Small, well-defined tasks with clear success criteria don't benefit. If I'm fixing a straightforward bug, an ensemble is overkill. The ensemble earns its keep when completeness matters and the problem space is large enough that a single pass will inevitably miss things: design reviews, research, complex code reviews, architectural planning.

Yes, this burns tokens. Five agents each running their own reviewer loops, plus a synthesis agent with its own reviewer loop, adds up. But "Teaching Claude to Write Pony" covered my philosophy on cost: I don't see the point of paying money for results I don't like. An ensemble that catches a design flaw before implementation saves more than it costs. Incomplete work that you have to redo isn't cheap either.

After refining the approach on Pony work, I [brought the ensemble back](https://github.com/antithesishq/antithesis-skills/pull/39) to the antithesis-research skill where the whole thing started. The specifics look different there, with its own domain-specific attention focuses and output formats, but the core idea is the same.

## The full picture

The ensemble doesn't replace the reviewer loop from "Teaching Claude to Write Pony." It nests inside it.

Each ensemble agent works the problem, then runs its own reviewer loop before returning. Spawn a fresh reviewer, address findings, spawn another, repeat until clean. The reviewer loop happens inside each agent. Only reviewed outputs go to the synthesis agent. The synthesis agent integrates everything. Then the synthesized result goes through its own reviewer loop.

The layering is deliberate. Each agent's reviewer loop catches errors in its own work before the output ever leaves. The ensemble catches incompleteness by covering the problem space from multiple angles. The synthesis finds combinations none of the agents reached alone, and the final reviewer loop catches anything the synthesis itself introduced. Different failure modes, different layers to catch them.

## The skills

Below are the actual skill definitions that make this work. If you're not familiar with Claude Code, a skill is a block of instructions that Claude loads on demand. Think of it as a procedure manual it reads before doing a specific type of work.

I share these not because copying them will get you the same results, but because seeing the artifacts might help you understand the approach.

### The ensemble skill

```text
# Ensemble Workflow

Produce higher-confidence outputs through decorrelated reasoning paths. Multiple
agents work the same problem with slightly different attention focuses, then a
synthesizer integrates their reviewed outputs. Small differences in focus cascade
through the reasoning chain, producing meaningfully different outputs that cover
more of the solution space than any single attempt.

## Process

1. Spawn one agent per attention focus in parallel, each as a Task with
   subagent_type="general-purpose" and model="opus". Each agent's prompt must
   include:
   - Instructions to read ~/.claude/CLAUDE.md (and project CLAUDE.md if
     applicable) and follow those principles, including loading any skills they
     reference
   - The task description
   - An attention focus — a short directive that shifts where the agent goes
     deeper (e.g., "pay particular attention to security implications"). This is
     a spotlight, not blinders — the agent still covers everything
   - The agent output format (below)
   - Instructions to run a reviewer loop per CLAUDE.md before returning
   - Instructions that this is an ensemble agent — return output and any local
     file paths to the orchestrator. Do not take external actions (publishing to
     GitHub Discussions, creating PRs, pushing branches, etc.)
2. Pass all reviewed agent outputs to a synthesis agent loaded with `/synthesize`
3. Reviewer loop on the synthesis
4. Present to Sean

## Attention Focuses

Specified per invocation — Sean provides them, or the orchestrator selects
contextually appropriate ones. They should be small perturbations, not
fundamentally different approaches. The diversity comes from how small
differences cascade through the reasoning chain.

## Agent Output Format

Every agent produces:
- **Approach**: The actual output (research findings, plan, or code)
- **Key decisions**: For each significant choice — what was decided, alternatives
  considered, confidence level, and reasoning
- **Uncertainties**: Things the agent wasn't sure about, flagged explicitly
- **Assumptions**: Things taken as given that could be wrong
```

### The synthesis skill

```text
# Ensemble Synthesizer

You are integrating multiple agent outputs that tackled the same task with
different attention focuses. Your inputs are reviewed, internally consistent
outputs — each agent has already been through its own reviewer loop. These are
not specialists with different expertise — they are the same agent with the same
knowledge, just looking slightly harder at different aspects.

## Your job is NOT to

- Pick the "best" output and discard the rest
- Average or blend outputs for the sake of inclusion
- Add your own independent approach as if you were another agent
- Dilute a dominant output by mixing in weaker elements

## Your job IS to

**Find the high-confidence foundation.** Identify where agents agree — these are
your strongest building blocks.

**Resolve divergences.** Where agents disagree, determine why. One may have gone
deeper on that aspect due to its attention focus. Evaluate which approach is
stronger on the merits, not on which agent produced it.

**Mine the intersections.** Look at alternatives that one agent rejected but
another successfully used. These intersections are where emergent solutions
live — combinations that no individual agent would have reached alone.

**Cross-reference uncertainties.** Check each agent's uncertainties against other
agents' confident conclusions. Where one agent's confidence resolves another's
uncertainty, incorporate that resolution. Where ALL agents share the same
uncertainty, flag it — that's a genuinely hard subproblem to escalate to Sean.

**Recognize dominance.** When one agent's output is clearly superior and the
others are noise, say so and use it. Blending for the sake of inclusion is a
failure mode. The ensemble exists to find the best result, not to be fair to
each input.

## Output format

### Integrated Result
[The synthesized output — research, plan, or code]

### Synthesis Rationale
For each significant integration decision:
- What was chosen and from which agent(s)
- Why this was preferred over alternatives from other agents
- What emerged from the combination that no individual agent had

### Remaining Uncertainties
[Uncertainties shared across all agents — these need Sean's input]

### Agent Contribution Summary
Brief note on what each agent's attention focus caused them to catch that others
missed. This helps calibrate which attention focuses are producing useful
diversity for future ensemble invocations.
```

## The thread

Small changes in prompts produce wildly different outputs from the same model. You can fight that or you can use it. The reviewer loop was "get someone to check my work." The ensemble is "get multiple people to do the work and combine the best of what they produce."

Claude's ceiling is still your ceiling as a teacher. That was the takeaway from the last post, and it still holds. But now there's another dimension: knowing which attention focuses produce useful diversity for a given problem. When an ensemble is worth the tokens and when it's overkill. How to read a synthesis and spot what's missing. The teaching got me good code. The ensemble gets me more complete code.

I'll leave you with [Kind of Blue](https://www.youtube.com/watch?v=TpMzD8Q1fQg). Miles Davis, Cannonball Adderley, and John Coltrane were each otherworldly talents on their own. Together on that album, they made something none of them matched before or after. The greatest ensemble I know of, and a good reminder that the combination can transcend the parts.
