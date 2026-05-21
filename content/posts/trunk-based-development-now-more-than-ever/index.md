---
title: "Trunk Based Development — Now More Than Ever"
slug: "trunk-based-development-now-more-than-ever"
date: "2026-05-21T07:00:00-04:00"
draft: false
tags: ["Software Development"]
description: "Live on main."
summary: "The pull request was never the quality gate we pretend it is. A tight loop on the trunk is the real thing, and that's truer now than it's ever been."
---

The first code reviews I was ever part of happened in a room. Someone's change up on a projector, the author walking us through it, the rest of us asking questions until we understood it. What does this do. Why this way. What happens when that's null. You got it, because the person who wrote it was standing right there, and you could push on anything that didn't sit right.

That looks like a different thing from what we do now. A diff on a web page, comments trickling in over a day or two, somebody clicking approve. Different decade, different tools. Strip the surface off, though, and it's the same thing. Review at the end. The change already built. The decisions already made. A bunch of people seeing it for the first time, long after the point where doing anything about it was cheap. The projector and the web page are the same mistake in different clothes.

One part is not the same, and it's a part that matters. A diff is just the change. The lines that moved, pulled out and laid on a page by themselves, with everything around them left off. You look at it from outside. The room worked the other way. We didn't review the change. We reviewed the codebase with the change in place, walked through the code with the new work sitting where it would actually live. You weren't looking at a change. You were in it.

Being in a change and looking at one are not the same thing. That's the whole reason the average review in a room ran a little ahead of the average on a web page. A little. Not a lot. And not because the room found the bugs. Maybe it caught a few more than a web page does. If I had to guess, I'd guess it did. It doesn't matter. Code review is sold on catching bugs, stopping them before they ship. So does it?

## The quality gate that isn't

Sometimes. Everyone's watched a review catch a real one. You've done it. Someone caught one of yours. That's real, and it's exactly the trap, because it works just enough to convince you it's worth it. Catch a bug in review on a Tuesday and you walk away sure the gate is doing its job.

Catching a bug now and then is not the same as being a quality gate. The compiler catches bugs. Running the thing once catches bugs. The question was never whether review ever catches one. It's whether gating on it is the most effective, time-efficient way to catch bugs, and whether the loop it drags along, the branch, the wait, the reviewing of code that's about to change anyway, costs more than it saves.

And the teams that gate on every change? They don't ship cleaner than the teams that don't. I've looked. It isn't there. If the gate did what it's sold to do, you would see it, PR-gated shops shipping clearly cleaner software than everyone else. You don't. Those same shops block releases for the sake of quality while production fills up with bugs all the same. Review works just enough to feel like a gate. It does not work well enough to be one.

The things that actually pull the bug count down are duller, and they hold. Building the software so the bug can't be expressed in the first place, the way a type system worth its salt won't let you write one. Testing it hard, the kind you design in from the start instead of bolting on at the end. I've written about [how I know it's working]({{< ref "how-do-i-know-its-working" >}}), and review isn't on the list. It never was.

So if it isn't really catching the bugs, what is the gate for? Trust. You don't trust people to write good tests, so you make them show you the diff. You don't trust them not to do something you'd have done another way, so you make them ask first. Pull the quality-gate costume off and that's what's standing there. A trust mechanism. The other things review does, spreading knowledge, teaching the codebase, settling how a team works, are real and worth having. The question is whether a gate at the end of the work is the best way to get them, or whether there are better ways.

"There's no other way to catch bugs," says the process leaking bugs into production. We keep gating because it works just enough to keep the story alive, and because doing anything else takes two things in short supply. Imagination, to picture a way that isn't this one. And nerve, to try something that doesn't already have industry best practice stamped on the side. It isn't that code review doesn't work. It's that it's a dumb way to catch bugs, and there are better ones.

## By the time you open the pull request

By the time the diff is on the page, the author has already made a hundred decisions. What to build. How to shape it. Which tradeoffs to take and which to refuse. What to leave for later. Days of decisions. Sometimes weeks. And almost none of it rides along with the diff. The diff shows what the code is, never why it's that and not the other thing. Unless you're on one of the rare teams that writes all of it down, the context that produced the change didn't come with the change. Most pull requests, even from a teammate you trust, tell you about as much as a drive-by patch from a stranger on someone's open source project. Which is to say, not much.

So the knowledge-moving review is supposed to do, the teaching, the better idea, the "wait, why are we doing it this way at all," lands at the worst possible moment, when it lands at all. After. After the work is done and the tradeoffs are set in concrete. Telling someone the whole approach is wrong when they're three weeks deep and asking to merge isn't feedback. It's a demolition.

I've written before about the [OODA loop]({{< ref "ooda-loop" >}}): observe, orient, decide, act, then do it again, fast, around and around. The pull request is the longest, slowest loop you can build. Someone takes a ticket and vanishes. No feedback, no contact with anyone else's thinking, until they reappear with something "done" and ask for review. That can be days. The whole time they're flying on decisions nobody else has looked at, and the looking, when it finally comes, comes too late to be cheap.

The fix isn't a better diff. The fix is to move the looking to the front. [Go slow to go fast]({{< ref "go-slow-to-go-fast" >}}) is the one I say most: write down what you're building and agree on it before anyone touches code. The more a piece of work is going to cost, the more the agreeing belongs up front, where changing your mind is free.

And it doesn't stop at the upfront agreement. I ask for review all the time. I just ask early. I've got something tricky and I'm not sure it's doing the right thing, so I grab someone: take a look at this with me. Sometimes before there's any code at all. Is this the right algorithm? Is this the right architecture? That's review doing real work, at a moment it can still change where the thing goes. Pull it in early, pull it in whenever you want another head on the problem. Killing the gate doesn't kill the collaboration. It just stops treating a sign-off as an oracle of quality. Nobody's approval makes your code right.

## Quality is not a gate

The whole gate idea rests on a belief worth dragging into the light. The belief is that quality is something you hold by keeping the bad out. Catch the bug at the door. It's a comforting picture. It isn't how good teams stay good.

You don't hold quality with a gate. You hold it by keeping the loop tight enough that when something's wrong you find out fast and fix it fast. Detect and recover beats predict and prevent. You are going to ship bugs. Everybody ships bugs. The teams that stay good aren't the ones with the tallest gate, they're the ones who notice quickly and fix quickly.

Accounting has a discipline worth borrowing. You don't erase a mistake in a ledger. You don't reach back and scrub a bad number like it was never there. You post a correction. The ledger only ever moves forward, the wrong entry and the entry that fixes it both stay on the books, and the books stay honest. Code can work the same way. Something goes in, something turns out to need improving, and you don't rip it out and pretend it never happened. You add the change that makes it right. Forward. Usually not even a dramatic revert, just a small fix to the part that needed it.

That's [baby steps]({{< ref "baby-steps" >}}): you move one small step at a time, each step leaving things better than you found them, and you don't freeze waiting to make it perfect before it lands. (Not the same as go slow to go fast. That one is about preparing before you start. Baby steps is how you move once you have.)

There is one place a gate earns its keep, and it's the only one. When you can't trust the person sending you the code, you need a gate. What you can't trust is their intent. An open source project taking a patch from a stranger has to assume the stranger could be hostile, could be slipping something in, could be the leading edge of a supply chain attack. You gate that. You'd be a fool not to. But a team of people who trust each other? An open source project among maintainers who trust each other? There's no stranger at the door. Gating there means you've been treating trusted people like strangers and calling it rigor.

## Living on main

I've worked the other way, and I want to tell you how it actually felt, because for me it isn't a thought experiment. No pull requests. We lived on main. All of us, on the same line of code, pushing to it many times a day. One trunk, everyone working on it, all the time. That's trunk-based development. We didn't call it that. We just called it working.

If you came up gating everything, that sounds like chaos. It wasn't. It was the room, except the room never ended. Instead of saving up a big change and walking everyone through it once at the finish, you integrated all day, and everyone was always looking at the latest code, because the latest code was the only code there was. You'd pull, and there was your teammate's whole morning, already in, already running. You didn't read a diff of it. You built on top of it. If it was strange, you felt the strangeness in your hands instead of reading about it on a web page.

Nobody called that review. It was. A whole team working the same code all day is review running in the background, ambient and constant. No gate to clear. No one's blessing to wait for. Everyone in the change at once.

When something was big, or might be contentious, we wrote it down first and came to agreement, and we were careful with the words so we couldn't talk past each other. When something controversial turned up mid-stream, we stopped and talked, right then, not in a comment thread three days later. And the second set of eyes everyone loves about code review? It was there the whole time. It was just attached to people standing in the running system with you, instead of squinting at a patch they had never run.

That's what the pull request takes from you and pretends to give back: another person engaged with your work. Trunk-based development doesn't get rid of the second set of eyes. It puts them somewhere they can see.

## How I know it's working

Which brings up the question that's been building since I told you that you're going to ship bugs. If nobody's gating, what keeps main from becoming a garbage fire?

Start with what I'm even after. I don't care about tests. I care about a working system. Tests are one way I find out whether I've got one. So is a type system that won't compile the bug. So is the thing running in front of me. The question I design around isn't "what tests do I write," it's "how will I know this is working," and then I build the answer into the work instead of bolting it on at the end.

The tests are one of those ways, and in a tight loop they run at two speeds. Fast ones, smoke tests that finish quick and run constantly, so the second you break something obvious you know it before you've moved on. Slower tests behind them: property tests, generative tests that throw conditions at the code you'd never think to write by hand, running in the background against recent main, digging for the latent stuff while you keep working. You pull, you put your change on top, you run the fast ones, you push. The slow ones grind away somewhere else. Or you run the whole pile locally, if your team isn't moving fast enough to need anything fancier.

Then there's the thing the branch-and-pull-request model quietly breaks. Work on a branch and your tests run against your branch: your change sitting on top of main as main was the moment you started. Green. Everything passes. Except by the time you merge, main has moved. Other people merged their work. The combination your tests blessed, your code against that older main, is a combination that never ships. What ships is your code against a main three merges further along, and nothing tested that. You got your green check on a fiction.

You can chase it, sure. Rebase onto the latest main and run again. But main keeps moving, and by the time your rebase comes back green it can have moved again, so you're chasing a line that won't hold still. Or you put a merge queue in front of main. That one actually tests the real combination, each change against the main it'll really land on, and it pays for the privilege by making the loop slower, every change waiting its turn. Living on main, there's no fiction to chase and no queue to wait in. There's only ever the real combination, integrated as you go, tested the way it actually is.

And in the large, what do you measure to know it's working? Not how many bugs the gate caught. You measure what happens in production, where the system works or it doesn't. You measure how fast you ship things that actually work. Those are numbers that mean something. "Pull requests reviewed" was never one of them.

None of this kills CI. It moves it. The old job was the merge gate, a hoop your branch had to clear before it was allowed in. The new job runs against the real main, continuously, and checks one thing: is what we've got, right now, safe to ship? Same tests, same work, aimed at a different target. That is what CI meant before we turned it into a merge gate in the first place: integrate early, integrate often, and let the tests catch the break the instant it happens. So the gate to production didn't disappear when the merge gate did. It just stopped being a person, or a green check on a pull request. It became the tests, running against the real main, all day.

## Now more than ever

Everything to here is true with no AI in the picture. It was true twenty years ago. It'll be true on a team of nothing but humans tomorrow. There's a new thing in the room now, though, and it makes the argument sharper instead of softer.

We've got code coming out of LLMs. A lot of it, fast. A coding agent can turn out in a few minutes what used to be a person's day. You can think that's wonderful or you can think it's a catastrophe, and it changes nothing about what comes next, because the code is being generated, in enormous volume, whether you welcome it or not.

Look at what people are reaching for to deal with it. The pull request. They take the agent's output, stand it up as a diff, and have a human squint at it before it merges. They took the model that was already bad at finding bugs and bolted it onto the end of a brand new firehose. It's worse here than it ever was with people. When a human wrote the diff you could at least ask them why. With the agent's there's no author to ask, so people bolt a second LLM onto the pull request to answer for the first, a machine explaining a machine's code to a human who then has to bless it. A reasonable problem met with an insane solution: more machinery piled onto the gate instead of a question about whether the gate belongs there at all. Either way you're reviewing code no person wrote, at a volume no review was built to handle. That isn't a quality gate. That's a person drowning.

If your gut says the code can't be trusted, that an LLM will hand you something broken with total confidence, good, I'm with you. But watch what that instinct actually argues for. I said a gate earns its keep when you can't trust the sender, and I meant a particular kind of trust: their intent. The stranger might be hostile. That is not this. You're not afraid the agent is an attacker. And if you were, you'd just close the pull request. No review needed. You're afraid the code might be wrong. Wrong is precisely what a tight loop is built to catch: the fast tests, the real integration, the system running in front of you. If what you distrust is the code's correctness, the gate is the wrong tool and the loop is the right one. Your distrust is an argument for living on main, not against it.

And it gets better than that. The things a tight loop wants, more eyes, fast checks run constantly, the slow ones grinding in the background, are things agents turn out to be unusually good at supplying. Want more eyes on a change? Start another agent and point it at the change. Want the fast tests run every loop? They're cheap, and an agent will run them every single time and never once get bored. Want the slow tests churning while the work goes on? Give an agent a worktree and let it churn. The throwaway spike, where you build the whole thing just to see what it would look like and then throw it away, used to cost a day. Now it costs a few minutes and some tokens. Everything trunk-based development asks for, agents can pour on by the bucket.

Agents fitting the loop is one thing. Trusting what they produce is another, and that part isn't solved. You still have to know the code is right. I can't tell you what finally cracks that, we're too early. But I know what I wouldn't build it on: the pull request, the tool that was already the wrong shape for us, that we grabbed for again the second the machines showed up. Whatever we build to validate agent code should look a lot more like trunk-based development than like the pull request: tight loops, continuous verification, a machine checking the real thing all day, fast enough to keep up with the firehose. Don't pour the new work onto the old broken foundation. Build it on the one that already worked. I can't prove that's right. It's just where I'd build.
