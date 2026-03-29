---
title: "Peter Alvaro and Me"
slug: "peter-alvaro-and-me"
date: "2026-03-03T16:00:00-05:00"
draft: false
tags: ["Testing", "Distributed Systems", "Antithesis"]
description: "I love it when a plan comes together!"
summary: "How Peter Alvaro's lineage-driven fault injection clicked with me, and why I ended up at Antithesis."
---

I've been [obsessed](https://www.youtube.com/watch?v=fZokPAuhD6k) with testing for a long time. Not "I think testing is important" obsessed. More like "I will corner you at a conference and talk about fault injection until you excuse yourself to get another drink" obsessed. So when I read Peter Alvaro's paper "[Lineage-Driven Fault Injection](https://people.ucsc.edu/~palvaro/molly.pdf)" years ago, it just clicked with me.

Here's the problem Peter was after. You've got a distributed system and you want to know what happens when things fail. Components crash, messages get lost, networks partition. The obvious move is to inject faults and see what breaks. But which faults? You can throw stuff at the wall randomly and hope you hit something interesting. Or you can try to be exhaustive about it, but the number of possible failure combinations in a real system is absurd. You're not going to enumerate them all. You'd grow old trying.

What Peter did was flip the whole thing around. Instead of starting from "what could go wrong" and working forward, you start from a good outcome and work backwards. You look at a successful result and trace how it got there. Which components were involved, what messages were passed, what had to go right. Then you start pulling those threads. What if this node had been down? What if that message never arrived? You're using the system's own behavior to figure out where the interesting failures are, instead of guessing.

I'd been thinking about testing in a similar way, but sloppily. Peter's paper took that and made it precise. It clicked with me. Boy-howdy, did it click with me.

Once upon a time in 2016, I gave a talk called "[How Did I Get Here?](https://www.seantallen.com/talks/how-did-i-get-here/)" about building trust in distributed systems. Some people think it was really just an excuse for me to talk about Buffy the Vampire Slayer at a tech conference. They're not entirely wrong. The talk was about Buffy, a distributed stream processor that would eventually be renamed Wallaroo, and Peter's paper was one of the key inspirations for how we approached testing it. We weren't using Peter's tool. What we did was far more primitive. We applied it by hand: start from the bad outcomes, figure out what could cause them, inject those faults, trace the messages, audit the results. It was primitive and we had a lot of stubbornness to make up for what we lacked in tooling.

In that same talk, I mentioned [Will Wilson's Strange Loop presentation](https://www.youtube.com/watch?v=4fFDFbi3toc) about deterministic testing at FoundationDB. If you haven't seen it, go watch it. I'll wait. The FoundationDB team had built a deterministic simulator for their database. They controlled all the nondeterminism: when threads got scheduled, when disk writes completed, when network packets arrived. They could replay any execution exactly and systematically explore different orderings to find bugs. I was envious. I wanted that. Because it was really cool. Because it would have made the question we faced at Wallaroo, "how do we know this thing is actually working," so much easier to answer. But building something like that is a massive undertaking, and we didn't think we had the runway for it. So we kept at it with what we had.

I don't remember when exactly, but at some point I was talking with Peter and he told me they'd tried applying lineage-driven fault injection in a production environment and the project had been abandoned. The state space was just too big. Even with the lineage-guided approach narrowing things down, there were still too many possible failure combinations at real-world scale.

That stuck with me for years. Here was this idea that really spoke to me, and the person who crystallized it was telling me it didn't work out in practice. Bummer.

I spent a lot of time trying to figure out how to write about what comes next without sounding like I'm hawking knockoff handbags on Canal Street. I can't. **shrug**. I'm a few weeks into my time at [Antithesis](https://antithesis.com). I'm excited about the product. I'm excited about working there.
 
Antithesis was founded by Will Wilson and a bunch of the same FoundationDB people who built that deterministic simulator I'd been envious of since I sat in the audience of Will's talk. They took FoundationDB's testing harness and made it into something you don't have to build yourself. Your software runs in their environment, deterministically. They run a ton of parallel "universes" for your application with different orderings, failures, and timing, and check whether the properties you've defined actually hold across all of them.

Last week I was talking with Lorin Hochstein about Antithesis. He was telling me about his day-to-day work. And yeah, those aren't testing problems, those are different. Actually... that's a huge state space. And before I'd really thought it through, I was telling him that Antithesis was exactly the tool for his problem. Not because of deterministic simulation or replay. Because it's a machine for exploring enormous state spaces.

I didn't think too much about it at the time. Some people like Black Sabbath because they have all these [weird, abrupt transitions](https://www.youtube.com/watch?v=0qanF-91aJo). Here's your Black Sabbath moment:

Peter. The state space was just too big. That's what he told me. And here I am, a decade later, at a company that built a machine for exploring enormous state spaces. Peter's insight was right. Working backwards from outcomes to find the failures that matter is still a great way to think about testing. The problem was never the idea. The problem was that the space was too damn big to get through. Look ma, new blog post! Don't get used to it.

You know that moment at the end of every A-Team episode where [Hannibal lights his cigar](https://www.youtube.com/watch?v=FPQlXNH36mI) and says "I love it when a plan comes together"? It's not really a plan here. But that's the feeling. Peter's paper. Will Wilson's talk. The testing work we did at Wallaroo where we were doing the best we could with less than what we needed. These have been rattling around in my head for years, and in the last few weeks they all connected. I love it when things come together.

One of the things Peter and I bonded over was our shared love of hip hop and 70s funk. I can't end a post called "Peter Alvaro and Me" without giving you [a Funkadelic song to listen to](https://www.youtube.com/watch?v=_20BvG3H6DY).
