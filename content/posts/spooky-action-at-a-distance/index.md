---
title: "Spooky Action at a Distance"
slug: "spooky-action-at-a-distance"
date: "2026-05-18T07:00:00-04:00"
draft: false
tags: ["Tropes", "Software Design"]
description: "Boo."
summary: "Non-locality is fine. Non-locality your tooling can't surface is the problem."
---

Anyone who's worked with me has heard me grumble about spooky action at a distance. The wording varies. The complaint doesn't.

Code in one place doing things to code in another place. Behavior that emerges from somewhere you can't see from where you're standing. The kind of thing where you change a function in module A and the type checker yells at you about module Z, and module Z hasn't been touched in months, and now you're sitting there trying to figure out what your change had to do with anything Z was up to.

I've been thinking about all the different shapes that complaint takes. There are a lot of them. Some are language features. Some are architecture. Some are the way a particular library decided to be clever. I always felt like they were the same kind of thing. I just couldn't quite name why.

I think I've got it now. Stick with me.

Take the visitor pattern. The data structure is over here. The operations on it are over there. Spread across as many visitor classes as you've got operations. Want to know what your code does to a particular kind of node? You find the data type. You find the visitor. You find the method on the visitor that handles that case. Three jumps. Different files. Probably different packages. The behavior is somewhere you have to go find. You can't just look at the type and see what happens to it. The type doesn't know. The visitors know. The visitors are over there.

Or Scala's implicits. You want to call a function. The function wants an instance of some typeclass. You don't pass it. Scala goes hunting. Looks in scope. Looks at imports. Looks at companion objects. Finds something that matches. Plugs it in. Lovely when it works. The function call gets shorter and the boilerplate goes away. Lovely until you wonder why the code does what it does. Now you're chasing imports and instance definitions across the codebase trying to figure out which instance Scala decided to use this time. Did somebody add an import three files ago? Did somebody define a more specific instance in a companion object? Could be either. You'll find out by reading.

This is dynamic scoping wearing a type system's hat. Dynamic scoping has been around since Lisp. People who've used it know the rhythm. A joy to write. A misery to read. The thing you're using in this function was set by someone, somewhere, at some point above you on the call stack. You can't tell from the code in front of you. You have to know the calling context, and the context that called that one. You have to know the whole program.

Implicits are dynamic scoping for typeclass instances. Same shape. Same trade. Easier to write. Harder to understand. The thing that decides what your code actually does is sitting in a file you didn't open.

Or global type inference. I love F#. I'll say that up front because the next thing I'm about to say will sound like I don't. I avoid F#'s type inference. I work not to use it. A team I was on for a while had a convention. Every function got type annotations on every parameter and on the return type. We did this voluntarily. The compiler doesn't require it.

Why? Because of the moment that everyone who has worked with global type inference has experienced. You add a new type. Call it `Bar`. You compile. Suddenly the compiler has decided that a function over in some other file, one that has been happily working with `Fu` for months, is now using `Bar`. Then so does another. And another. The inference walked the program, made decisions in light of `Bar`'s existence, and you're staring at a stream of WTF compiler errors that didn't exist five minutes ago. Code you didn't touch. Errors you didn't earn.

You added a type. The compiler did the rest.

That isn't a cognitive-load complaint. That's the thing actually moving. Add a type here. Break the world over there. The break is real and the cause is at the other end of the program from the symptom. That's spooky. That's the literal version of the title.

The team's convention, annotate everything, turned the language into something else. Inference still happened inside function bodies, where the value you're inferring about is sitting right there. Inference stopped at the function boundary. Sylvan Clebsch, who designed Pony, calls this *type implication*. The compiler can figure out what's going on locally. It doesn't get to propagate decisions across boundaries you'd want stable. F# with that convention applied is, in effect, a language with local type inference. Pony has always been that language. Same idea. Different starting point.

Spooky action doesn't stop at the edge of a program. Let's talk about message queues. Everyone loves message queues. Almost every leet architecture design now is a series of queues. We do this on purpose, and often for excellent reasons. Two services that used to talk to each other directly, now they talk through a queue. The producer doesn't know who's reading. The consumers don't know who's writing. We get decoupling. We get back-pressure smoothing. We get a buffer when one side is slow. We get a lot of nice things. We also get this. When somebody changes how the producer writes its messages, you can no longer easily know what's downstream. You don't know who's reading. You don't know what they're doing with what you send. The shape of your message is part of an API contract with parties you can't enumerate.

Pull on this thread far enough and you find that "I changed our payload format" and "I broke five things" aren't the same sentence in the same project anymore. The five things are over there. You don't even know where "there" is yet. You'll find out.

All of that probably sounds like I'm anti-non-locality. I'm not. Sit right down and let me tell you a story about what I'm actually against.

A while back I was building a system at a company. I was new there. The core idea of the system was that customers needed to give us data in their own shape, and we needed to understand it and work with it. So we were building, in effect, a type system for arbitrary customer data. The shapes were customer-defined. The storage was dynamically generated to match each shape. Data coming in got validated against the shape so the rest of the system could trust it.

I started in Go. I had the political capital of a newcomer with a good reputation, aka a bit, but not a whole lot. There was already a lot of Go in production. It was a safe choice. A choice that preserved my limited political capital.

I reached for the visitor pattern. I'm not a huge fan but sometimes it is the right choice. There and then, in Go, it was the right choice. It took me less than a week before I realized I didn't want to be on call for this thing. And I definitely didn't want to make someone else debug a codebase where everything happened "over there" at 3 am. The visitor pattern is great if you know how everything is put together. If you don't, you'll spend hours chasing visitors at 3 in the morning when you're tired and you didn't write the code.

It wasn't a wasted week. I knew what the shape of the core of the system was going to be. And I could go looking for a language that made that shape a joy to work with and maintain. F# has discriminated unions and exhaustive pattern matching. You define your type with its cases in one place. You match on it, and the compiler tells you when you've forgotten a case. Add a new case to the type, the compiler walks the whole program and points at every match expression that needs to handle it. That was what I wanted. Local definition. Local matching. The compiler doing the chasing for me when I made structural changes.

It cost. F# is niche. Nobody on the team had used it in production. I'd never run .NET in production anywhere in my career. I'd written a few toy F# programs years before and that was it. But I've often picked "the right tool for the job" at many jobs so the idea of using a new language that I'd never used before didn't scare me. Still, I was about to spend a lot of political capital.

A year later we needed to redo the type system. It was easy. A few months after that we added something we hadn't planned for, a kind of optional type we called "maybe" types. Ridiculously easy. The thing we'd taken the risk for, easy reasoning and a compiler that surfaces every place a change has consequences, paid out exactly as I'd hoped.

F# sum types and exhaustive matching produce, in software terms, a lot of non-locality. Add a case to a type and you've created consequences spread across every place that matches on it. Wide blast radius. Lots of code, far from where you made the change, has to do something different now.

Non-locality isn't what bothered me about the visitor pattern. It isn't what bothers me about global type inference. It isn't what bothers me about the queue. Those things also produce consequences far from where you made the change. So what's the difference?

I'm not the one doing the chasing. The compiler is.

When I add a case to an F# discriminated union, the compiler walks the program and tells me exactly where the consequences are. I don't have to chase. I don't have to know. The tooling chases for me. The action is at a distance and the action is loud. I can see it. I can act on it. I'm not the one doing the searching.

*Spooky is not the same as non-local.*

That's what I'd been trying to name. I don't have a problem with consequences happening far from the change. I have a problem with consequences happening far from the change that I have to find by hand. Non-locality is fine. Non-locality the tooling can surface is fine. Non-locality the tooling can't surface is what I've been calling spooky action at a distance.

This explains why I love Smalltalk and don't really like Ruby. They share a lot. Highly dynamic. Open to runtime modification. Code that does things you can't see from where you're standing. In Ruby, that's an active hazard. Open classes, `method_missing`, metaprogramming everywhere. A method gets called and you have no idea where the implementation lives. Maybe a gem you imported reopened a base class. Maybe someone defined a method dynamically in an initializer. There's no static fact for tools to grab onto. The tooling can't catch up because the program won't sit still long enough to be inspected.

Smalltalk is just as dynamic. More so, in many ways. The Smalltalk environment is built around interactive inspection of a live system. You want to know who calls this method? Ask the image. You want all the implementors of this selector? Ask the image. You're standing inside a system that is, by design, interrogable. The non-locality is everywhere. It's also discoverable. The tooling fits the language because the language was built with the tooling in mind.

Same family. Different choices about what the tools get to see.

My friend Andy would often complain about macros and how much he disliked them. I always told him he didn't dislike macros. He disliked the lack of tooling that the language was giving him.

This also explains why I'm fine with one kind of type inference and against another. Generic type inference at the call site is great. When I write a function call in C++ and the compiler figures out the template parameters from the arguments, that's local. The inputs are sitting right there. The cost of looking at the call to understand what got inferred is one glance. Before `auto`, holding the result in a variable meant typing the full template type. Iterators half a screen long. C++ with `auto` is a different language. The scope of inference is small, the inference happens where the values live, and reading the code doesn't require imagining what the compiler decided about something three modules away.

Global type inference is the opposite. The scope is the whole program. Where the values live and where the inference about them propagates can be on opposite sides of the codebase. Same axis. Scope of inference. The narrower the scope, the less surface for spooky behavior. The wider the scope, the more the tooling has to do to keep the consequences visible. F# can be either, depending on how you write it. C++ with `auto` is local by default. Pony is local by design.

Every one of these is a real trade. Implicits make some code beautiful. Queues make some systems possible. Global type inference makes some prototyping faster. The benefits are real. The costs are real too. People with enough experience know both. I carried a pager for years. My idea of beautiful code is code that does what it is supposed to and never wakes anyone up at 3 am. And if it does, it's easy to understand. So, I carry a lot of priors into where I place my trade-off chits.

The Agile Manifesto says it the way I want to say it. We value working software over comprehensive documentation. As a *comparison*, not a rejection. Both sides of the trade are real. Calling the trade out loud is how engineers in their big boy and big girl pants talk about choices. Me? I value local reasoning over write-time terseness. I value visible non-locality over hidden non-locality.