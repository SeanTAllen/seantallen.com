---
title: "Go Slow to Go Fast"
slug: "go-slow-to-go-fast"
date: "2026-05-07T02:00:00-04:00"
draft: false
tags: ["Tropes", "Software Design"]
description: "A reminder."
summary: "Slowing down isn't just allowed. It's the faster path."
imagePosition: "center 58%"
---

Long, long ago...

I was working at a small online retailer, redoing its order system. We split the work into two streams so we could move independently and skip the coordination overhead. We'd build in parallel, snap the modules together at the end, and victory would be ours. Friday before Super Bowl Sunday, we plugged them in. Module A had pulled in version 2 of an HTTP library for its server. Module B transitively pulled in version 1 for its client. Same namespace, can't coexist. I spent the weekend forking the dependency that needed v1, porting it to v2, testing it, and stitching everything back together. Friday night, all of Saturday, deep into Sunday. The Super Bowl came and went without me. Good thing I didn't care about that particular game. We hit our Monday deadline.

I was working at a large web property where engineering bonuses were tied to velocity. Everyone tried to cram as many story points into an iteration as they could. Weekly release rolled around. A member of my Scrum team slid his feature in under the wire. Code hit production. Splunk lit up. Uncaught exceptions everywhere. He rushed a fix. The company hadn't invested in fast deploys, so each rollout took 45 to 60 minutes. We watched the exception count climb while we waited. The fix shipped, the exceptions calmed, and a new wave replaced them — because to land the original feature on time, he'd copy-pasted the same logic into a few other places. Each one broken the same way. Another hour of broken production queued up while he prepped the next patch. I cluster-sshed into the production machines, opened the JVM bytecode, and patched the three sites directly. The bug was a `>` that should have been a `>=`. A single bytecode instruction. Done in seconds.

I was running a small ML group. We did matching of jobs to jobseekers, and jobseekers to jobs. We had a short window over the slow holiday stretch to move the document store from Solr to Elasticsearch. We could do plenty of pre-work, but the cutover itself meant three of us — L, P, and me — rotating overlapping twelve-hour shifts for several days so we'd be done by Christmas Eve and could enjoy our holidays.

We did it. Loopy, exhausted, and proud of ourselves.

Over the next few months, occasional weird-results reports would surface. "These jobs don't match me." We'd open the model, look at the features, and find a story that explained it. Plausible enough. We moved on.

Then one report came in that made no sense. None. I was sitting with P, doing what I do, which is make jackass jokes when something is broken. I said, "You know, these results would make sense if we were tokenizing on the letter A."

A throwaway line. Meant nothing.

A short pause. Then P said, loudly, "Fuck."

"What? I was just being an ass."

He kept saying it. Mumbling it. Less than five minutes later we had it.

During that push to Christmas Eve, P had asked me, "We want to keep the same tokenization rules, right?" I said yes. We copied the rules straight from the Solr config into the Elasticsearch config. Harmless. Except Solr's config is XML and Elasticsearch's is JSON. In XML, `&` has to be escaped as `&amp;`. We copied the `&amp;` straight across. Elasticsearch doesn't unescape XML entities. So instead of tokenizing on `&`, we were tokenizing on `&`. And `a`. And `m`. And `p`. And `;`.

For months. Turns out we were tokenizing on `a`.

The executive meeting where we walked through the "occasional weird-results issue" was a hell of a thing.

That was a moment. The idea was born there. The phrase came later. Not that day. Not right after either. But not long after. Whatever I said back then was longer. Clunkier. It took saying it out loud, over and over, to find the four words. To that team. Then to every team I ran. Then to anyone who'd listen. Shorter every time. Tighter every time.

*Go slow to go fast.*

It means writing a planning document and coming to agreement on what we're doing before anyone touches code. The document doesn't have to cover everything. It captures what we agreed we're doing, and it gives us somewhere to think through the ramifications and the edge cases.

It means knowing how we'll test the thing before we start building it. Designing it to be easy to test, instead of bolting tests on after the shape is locked in.

It means taking a few hours, or a couple of days, to do a throwaway spike on the parts we don't understand yet. Probing for unknown unknowns before they bite us at the worst possible time.

It means stopping when production is on fire at 2 a.m. Taking stock of what we actually know and what we're just guessing at. Acting on an untested hypothesis in the middle of an incident is how a bad situation gets worse.

Less than two years after the tokenization disaster, I was designing Wallaroo, a high-performance distributed stream processor. We started deliberate. What are our goals? How will we test this? How will we know it's working? We invested in the foundation. First, we built a slow, throwaway "Wallaroo" in Python. We built the first version of our test harness against it. Then we started on the real Wallaroo. The fast one. The Pony one that was going to make us rich. We built a chaos-inducing fault injection system before we built the features that would need it. We picked Pony, a language whose compiler refuses to let us write data races. None of us had used Pony. We slowed down to learn it. Weeks of ramp time, in exchange for never writing a class of bug we'd otherwise spend years chasing in production. We bought speed later by spending time up front.
