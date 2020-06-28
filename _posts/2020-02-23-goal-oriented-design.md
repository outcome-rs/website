---
layout: post
title:  "Goal-oriented design"
description: "Coming back I'm trying to think critically about some of the design aspects of the project, especially in regards to the core code. One of the most important things to do right now, in my mind, is to take a step back and try to be more specific and goal oriented with the design."
tags:
  - devlog
permalink: "/blog/goal-oriented-design"
published: false
---

<meta property="og:image" content="https://images.unsplash.com/photo-1484910292437-025e5d13ce87?ixlib=rb-1.2.1&auto=format&fit=crop&w=788&q=80"/>

As mentioned in the [last post]({{site.url}}/blog/back-to-work), coming back I'm been trying to think critically about some of the design aspects of the project, especially in regards to the core code. One of the most important things to do right now, in my mind, is to take a step back and try to be more specific and goal oriented with the design.

In the beginning I just wanted to learn stuff and didn't really care that much for rationale behind the overall design. As time went by and some of the ideas started to come together it became clear that there are decisions to be made. 

As it happens, specifying a concrete set of goals for your software can get complicated, especially if you've been kind of working on it on and off for a longer amount of time and there are lots of ideas floating around.

In this post I outline the most important changes to the core library that I intend to focus on.


## Focusing on a coupler approach

There is a certain term floating around the academia these days, namely something called [*co-simulation*](https://en.wikipedia.org/wiki/Co-simulation). From what I gather, it's basically a fancy way of saying that you're trying to create a system containing a bunch of smaller sub-systems, where each of those sub-systems can be treated almost as a *black-box*, with only very simple inputs and outputs being exchanged between them. This idea underlines some of the stuff that I've been planning on implementing with this project.

Of course what I'm definitely not planning on is getting into the academic-grade science here. I'm also not thinking about trying to design around existing standards, as they're just too complex. Remember, the approach I'm taking here is a fairly minimalistic, if not a simplistic one. Citing existing ideas and approaches I'm more trying to get a better grip on how to what's already been created, and on how to articulate the things I want to accomplish with this project.

The idea of creating a way for easily coupling multiple different precompiled libraries, standalone programs (working as servers/services, possibly being run on different machines), and smaller scripts and algorithms, is at the core of what I'm after here.

## Give up trying to prematurly optimize the orchestrator

Related to the idea of focusing on building a sort of "glue" framework, an orchestrator for multiple different simulation parts and solutions, is the problem of code efficiency.

In the past I've been considering experimenting with things like code generation. Right now I would say this is no longer on the roadmap.

The goal with so called orchestrator approach, where the main loop the engine provides running scripts that are written for it, is to nicely tie things together, and do so in a user friendly way. Speed- of efficiency-wise logic execution on the engine runtime will never compete with the speed of compiled rust or c code. And that's fine.

If some sort of community were to develop around this project, it would be best to let people implement different parts on their own using different tools and programming languages. The main coupler should provide a way to quickly prototype, design and test things on a higher level, making use of different existing solutions to create new models, games and experiments.


## Changes to user scripts

So far my approach towards processing was a simplistic one, in that I just went with a barebones list of commands, with only flow control available being simple jumps between them. This approach was wrong, in that it didn't fulfill the goal of providing an easy to use interface for quick prototyping.

All data loaded by the program was static too, formed into `yaml` files. Allowing the model assembly to be a dynamic process, including some kind of *preprocessing* ability, is a key change that will be happening.

Scripts should allow for at least some basic flow control like if-else statements, for loops and simple callable functions. Furthermore user should have access to a local variable store.

Custom scripts processed by the engine will probably end up resembling shell scripts, as that's probably the closest thing to what I'm looking for.

### Shell scripts? So is this a shell-like thing now?

Indeed the shell concept could potentially be useful when talking about the nature of the core sim engine here. You could describe it as a portable system with with built in data storage and procedure queueing system, that you can attach to and run commands on.

"Why not just work on the operating system level then" you might ask. "Why not assemble your models like a normal person would, using existing tools.". Well, that's a good point, and most of the serious work on simulating stuff happens this way. With this project things look a bit different though, as the goal is to create a relatively self-contained and relatively safe program that can be shipped with other programs, like games. It's meant to run on existing systems without making changes to them or require them to install any particular dependencies on the system level.


## Focusing on distributed processing

One of the most important "selling points" of the core sim library is that it would provide a solution that is not only multithreaded by default, but also make execution on multiple machines easy, if not seamless.

As such it will need to become a higher priority than it has been so far.


