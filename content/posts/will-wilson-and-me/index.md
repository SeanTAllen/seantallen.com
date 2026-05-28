---
title: "Will Wilson and Me"
slug: "will-wilson-and-me"
date: "2026-05-27T20:45:00-04:00"
draft: false
tags: ["Testing", "Wallaroo", "Antithesis"]
description: "Simpatico."
summary: "How a talk a friend pointed me to became a decade of simpatico with Will Wilson, and how that turned into a job at Antithesis."
imagePosition: "center 15%"
---

Years ago, I was at Strange Loop. A friend told me that I should come see this talk a friend of his was giving. I didn't know the speaker. I went because someone I trusted said it was worth my time, which at a conference is most of the reason you end up anywhere. Going where someone said I should at Strange Loop turns out to be pivotal in a number of important events in my life. But those are stories for another day.

That Strange Loop talk was [Will Wilson's](https://www.youtube.com/watch?v=4fFDFbi3toc), and it was about how FoundationDB tested their database.

FoundationDB built a deterministic simulator. That sounds dry. It is the opposite of dry. They took all the things in a distributed system that happen in whatever order they happen in, when a thread gets scheduled, when a disk write lands, when a network packet shows up or never does, and put every one of them under their own control. Once you control all of it, you can run your system, watch it do something interesting, and replay that exact run any time you like. You can take one run and fork it a thousand ways. Change the orderings. Drop the messages. Kill the nodes. Go hunting for the version of the run that breaks.

It would make a great story if I could say that the talk immediately changed my life. But it didn't. It is a hell of a talk. And it is ridiculously cool. And I would remember it in a way that most talks don't get remembered. I've seen tons of talks. I remember almost none of them.

Within a couple years, I was at a new job and everything from Will's talk became incredibly important to me. I was building [Wallaroo](https://github.com/SeanTAllen/wallaroo), a high-performance distributed stream processor. (It was named Buffy first, then Wallaroo, and after I left and the company pivoted it got renamed Wally to spare the market some confusion. Naming is hard.) Wallaroo was all about correctness. People were going to run their data through it and trust the answers that came back out. So the question that hung over everything was easy to say and miserable to answer. How do I know this thing is actually working?

Will's talk was an answer. A great one. It was also an answer I couldn't afford.

Building a deterministic simulator like FoundationDB's is an enormous undertaking, a project unto itself. You build the simulator first and the product second. We never had the runway for that. We always needed to show progress to keep the money coming in, and to the people writing the checks, all that upfront investment doesn't look like progress. So we didn't build the simulator first and the product second. We built the product and a testing harness side by side, at the same time, and we built the harness in the "inspired by" category rather than the "real thing" one.

What we built fed data into a Wallaroo application, caught it on the way out, and checked it for correctness. At first we'd take a whole run and analyze it after the fact. Later we did it as a stream, while the system was still running. And we had a stack of ways to inject faults, to reach in and simulate the failures we figured we'd hit out in the world. It was nowhere near what Will had shown at Strange Loop. But it was ours, it was the best we could do with the runway we had, and it earned back every bit of the considerable engineering we sank into building it, maintaining it, and chasing its bugs.

Will's talk, along with [Peter Alvaro's lineage-driven fault injection]({{< ref "peter-alvaro-and-me" >}}), is a big part of how [we came to think about testing at Wallaroo](https://www.seantallen.com/talks/how-did-i-get-here/). Those years are where I really learned the two things I care most about in testing now. [How to know whether the thing is working]({{< ref "how-do-i-know-its-working" >}}). And [how to design a workload worth running]({{< ref "/interviews/bug-bash-april-2026" >}}).

That is what I took from Will's talk. What I took from Will himself came slower, over a much longer stretch of time.

For years after Strange Loop, Will and I stayed in loose contact on Twitter. Not friends, exactly. But not not friends either. Friends in that way you can be friends with someone you have only barely met in person, but interact with fairly often.

Twitter is short form, and short form is acid on nuance. It strips the qualifications off an idea and leaves the hot take standing there alone. A lot of people on there, meaning to or not, end up posturing, staking out positions harder than they'd ever defend to your face, reaching for whatever framing starts the most fights. The loudest version of a thing is the version that travels.

Will wasn't that. In the middle of all of it he was grounded and steady. We never had the big, long conversations. There's no single defining exchange I can point you to. It was just the slow accumulation, one small interaction at a time, of the sense that here was a person I was simpatico with. That we saw things the same way. That his values and mine lined up. You can't manufacture that. It settles like sediment, or it doesn't.

Will made it onto my "maybe someday we find a way to work together" list. It's a small list. And a good one. If you ask real nice, I might show it to you someday. Maybe.

So years later, when a message from Will showed up in my LinkedIn asking whether I'd be interested in a job at Antithesis, it wasn't coming from a stranger. He sent along a link to their post in a Hacker News "Who is hiring?" thread. The gist was Jepsen-like work, the business of finding all the ways a distributed system will lie to you about being correct. It sounded like fun. I told him so. One thing led to another, and here I am.

Here is the part I still find a little ridiculous. At [Antithesis](https://antithesis.com) I get to play with a testing tool that looks an awful lot like the system we used to daydream about at Wallaroo. Except it isn't the daydream. It's well past it. Antithesis runs its own hypervisor. It will test arbitrary systems, not one company's one product. It's years of work in its own right, an actual product, where our harness was a thing we built on the side to keep our own product honest. I spent a real chunk of my life building and babysitting that harness because we were selling correctness and had no other choice. I'd have given a lot to have had Antithesis.

But the tool isn't really why this feels like a circle closing. The reason is that the place runs on the same things that kept me talking to Will for a decade. Grounded. Steady. Allergic to posturing. A building full of people who think the way he does. I spent ten years recognizing that in one person, one small interaction at a time. Now I get to do my work surrounded by it.

My first couple days at Antithesis, I was digging through various company "culture documents". So many of them were obviously written by Will. There it was, a company embodying the things that Will and I had discussed back and forth in public and private on Twitter. It's a good place to be. A good place indeed.

