---
title: "GitHub Is a Gift"
slug: "github-is-a-gift"
date: "2026-06-23T16:00:00-04:00"
draft: false
tags: ["Stories"]
description: "GitHub has taken a beating lately, but I'm old enough to remember when open source developers scrounged for the machines to test their code. From there, free CI looks like a gift."
summary: "I remember the before times."
imagePosition: "center 25%"
---

Here's how it was. Back in the before times, those of us writing open source software had to scrounge for the machines to test it on. There were a pile of competing Unix variants out there, and the expectation was that your software ran on all of them. They shared a core of sameness, but each had enough difference that getting your software working on one often bought you little on the next. Getting your hands on a single one of them was hard. Getting your hands on more than one was harder. All of it was particularly painful for me, because I was crazy enough to be writing threaded servers, and pthread support was wildly different across the Unixes.

To test across all of that you needed a build farm, and a build farm was a precious commodity. The NetBSD folks had one. A handful of other projects had one too. They were never plentiful. The way I remember it, even the Free Software Foundation didn't run its own. They leaned on whoever would have them, same as the rest of us.

Compared to now, there weren't many of us developing software "in the open", and when you can't get at the machines to build and test on, you end up with copious free time. We spent ours arguing. GPL or BSD. Which license was the one true way forward. "Open source" wasn't even the phrase yet, and the fight between free software and open source was already going strong.

If you wanted to really put your software through its paces, your best bet was to get it accepted into the GNU project or something like it that ran its own farm. Or you did what I did. You knew somebody working on NetBSD, you asked nice, and they spared you some odd cycles so you could see whether your stuff actually worked.

I'm telling you all this because GitHub has taken a beating lately. The uptime has been abysmal. They got the hockey-stick growth in demand that agentic coding dragged in, and they weren't ready for it. And look, I love to beat on GitHub. They've botched plenty of features over the years. But I remember the before times.

I'm incredibly thankful to GitHub for providing open source projects with free, almost unlimited CI. I can't imagine how much work I'd have to put in to maintain [Pony](https://www.ponylang.io) on all the platforms that I do without the gracious gift GitHub gives everyone "for nothing," simply because their code is in the open.

I'm not putting GitHub on a pedestal. I'm not calling them altruistic. I'm saying the world of open source development changed wildly because of them, and because of Travis CI before them, and because of every other outfit that handed us something for free. I'm old enough to remember the before times. And I know I'm blessed by what I get now.
