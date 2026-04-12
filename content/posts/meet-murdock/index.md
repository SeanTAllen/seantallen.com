---
title: "Meet Murdock: The Wildcard in the Ensemble"
slug: "meet-murdock"
date: "2026-04-12T08:00:00-04:00"
draft: false
tags: ["LLM", "Ensemble Method"]
description: "Why every ensemble of fixed-lens personas needs one deliberately unconstrained reviewer."
summary: "Every good team needs its wildcard."
---

Like most American men of my age, I spent my middle school years watching the A-Team. I loved how suave and confident Faceman was. Hannibal always pulled an incredible plan out of his ass and finished every episode looking cool with that victory cigar in his mouth. And B.A.? Mr. T was so damn cool that you couldn't help but root for Clubber Lang to beat the shit out of Rocky. And then there was Murdock. I didn't really pay much mind to Murdock. He was just there as the 4th character that was just kind of there. Was he supposed to be comic relief? I guess. If he was, it didn't land for me.

From time to time, I've come back and watched the A-Team again. Like most TV shows from my youth, it doesn't really do it for me now. But now, my favorite character is absolutely Murdock. You know each episode what you are getting from the other members of the team, but you never quite know what you are going to get with Murdock. He's the essence of "crazy like a fox".

What does all this have to do with anything? Well, the other three each had a role. Smooth talker. Planner. Muscle. You knew which role was whose before the cold open was over, and the show worked because of it. Murdock didn't fit a role. Or rather, his role was that he didn't fit one. The show needed him there for exactly that reason.

Murdock is a wildcard. And he was the inspiration for my favorite LLM persona that I've added to some of my skills. Bet you didn't see that coming. Anyway, let's dig in.

## Agentic Personae Redux

In [Agentic Personae]({{< ref "agentic-personae" >}}), I wrote about building persona sets for recurring LLM tasks like code review, design, and test planning. Each persona has a fixed lens. The Correctness reviewer traces code forward, following each branch and error path to verify the code does what it claims. The Adversarial reviewer works the other direction, starting from "what would a bad outcome look like?" and constructing a concrete scenario that produces it. The Security reviewer thinks in terms of trust boundaries, watching every place untrusted data meets trusted code. The Tests reviewer applies counterfactual thinking: if the code under test were broken in the way this test targets, would the assertion fire? Same artifact, different modes of thought. Each persona does a first pass independently, the synthesizer combines them, and the result covers ground no single pass reaches.

That's the baseline. If that's all new to you, Agentic Personae has the full setup. For this post, all you need is the idea that a persona is a specialist with a fixed lens.

## Meet Murdock

The wildcard is a persona too, but it's the only one without a fixed lens. It gets the same artifact as the specialists, and it gets the identity statements of every other persona running on the task. Not so it can coordinate with them. So it knows what territory is already covered and can avoid it.

Then it's told to look elsewhere.

The wildcard is permitted to question the premise that the other personas accept. Correctness assumes the change is trying to do the right thing and checks whether it does it right. The wildcard can ask whether we're doing the right thing at all. It's permitted to report vague signals — things like "this feels off, here's my best attempt at why." The other personas produce articulate, categorized findings. The wildcard sometimes produces half-articulated instincts. That's by design.

So that's what the wildcard is. Now for the part I actually care about.

### Why the wildcard exists

Every fixed lens has blind spots, and the blind spots are shaped like the lens. Categories are how specialists get good at their category, but the category is also a fence. The Correctness reviewer won't flag a design assumption nobody questions. That's not correctness. The Security reviewer won't notice a missing user workflow concept. That's not security. The Tests reviewer won't catch a specification gap. That's not testing.

You can add more specialists. More lenses cover more categorized territory. But the stuff that doesn't fit any category stays invisible no matter how many categorical lenses you field. If your review team is seven specialists and nobody else, a real problem that doesn't look like any of their categories slides past all seven. Not because they're bad at their jobs. Because their jobs are defined in a way that excludes it.

The wildcard is how you cover the uncategorized. Its job is the shape of a hole: whatever the other personas structurally can't see. It's explicitly unconstrained because the whole point is that constraints are what produced the blind spot.

That's also why vague signals are welcome. The specialists produce articulate findings because their category tells them what articulate looks like. The wildcard is working without a category, which means it sometimes knows something is wrong before it knows why. A half-articulated flag on a real problem is worth more than a fully-articulated flag on nothing. The specialists produce the latter all the time. The wildcard is permitted to produce the former, and when it does, I pay attention.

## One more thing

This post would be better with examples. The wildcard's findings are regularly stand-outs. I should have saved them. I did not. I was too busy grooving with Murdock to write down what he was doing.

Fair trade. You know each episode what you're getting from Hannibal and Faceman and B.A. That's the point of them. You never quite know what you're getting from Murdock. That's the point of him too.

## Oh, and one more thing

I always thought it was Murdoch. It wasn't until I went to write this post that I learned it's Murdock. Stupid internet. Ruining everything again.
