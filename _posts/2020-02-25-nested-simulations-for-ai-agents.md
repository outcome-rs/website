---
layout: post
title:  "Nested simulations for ai agents"
description: "Here are a few thoughts on nested sub-simulations and how somewhat-intelligent agents could be developed within the proposed framework."
tags:
  - devlog
permalink: "/blog/nested-simulations-for-ai-agents"
published: false
---

<meta property="og:image" content="https://images.unsplash.com/photo-1516476675914-0160447c7282?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=735&q=80"/>

According to the current state of science, part of our human intelligence is related to being able to create *simulations* of the world, using those to decide on actions that we take in the present. We're able to imagine future states of the world, to compute possible outcomes and focus on how to achieve those states that we want to see happen.

Although it's speculative and wishful thinking more than anything else at this point, I think there is a seriously cool potential for recursion - namely, given the design goals for the project, where models are run without a longer compilation process but rather get interpreted at runtime, and where they can be also changed and tweaked at runtime, one could make a case for running simulations within the simulations. 

Simulated entities, agents, that are supposed to exercise somewhat intelligent decisions could instantiate new simulations themselves. These *sub-simulations* would be scaled-down versions of the whole thing, focusing on solving specific problem, or set of problems, that the agent wants to know the solution to. 

To get any meaningful answer sub-simulations would have to be run multiple times. This is the idea of *proofs*, that I've already started exploring in the past (and definitely want to advance going forward). It's about getting meaningful answers about a complex system, based on multiple simulation runs where for each run we introduce some specific tweaks. 

So say that you have a complex city system and you want to get an answer to the question on whether industrial pollution has an impact on people's health. You run 5 simulations, each with industrial pollution value set to different levels, and you check the results. Of course this is barely useful for these sort of super-simple thought experiments like this one, because we can be quite confident of the result without running any simulation at all. Once you try to evaluate more input changes and are dealing with more complex problems though it starts to become quite helpful.

When it comes to agents and iteractions between agents, perhaps the most interesting application for sub-simulations would be the situation where agents try to guess each others' behavior. They could go to the next nesting level and try to simulate another agent's behavior *inside* a sub-simulation *inside* the main simulation process. Woah.

Of course all of this is likely to pose a huge performance problem. Any simulation dealing with thousands of entities where each one regularily runs thousands of shorter sub-simulations on it's own is likely to be too heavy for any consumer machine. That's why being able to distribute the computation over multiple machines is so important. Given we don't care for super fast tick-rates, as sending data between multiple machines scattered all over the place would not allow those, we should do just fine getting more computers to do the work.
