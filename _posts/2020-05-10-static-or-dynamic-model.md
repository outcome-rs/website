---
layout: post
title:  "Static or dynamic model?"
description: "There is a potentially useful distinction between a static and a dynamic model. How are they different? In general terms we could can define computation as simply doing work on data."
tags:
  - devlog
  - engine
  - design
permalink: "/blog/static-or-dynamic-model"
published: false
---

<meta property="og:image" content="https://images.unsplash.com/photo-1512439408685-2e399291a4e6?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=530&h=300&q=80"/>


There is a potentially useful distinction between a static and a dynamic model. How are they different?

## The philosophical question

In general terms we could can define computation as simply doing work on data. Based on that we could say a model is just the totality of the data we feed into the computation.

In practical terms though, we usually end up differentiating between the highly variable and the more stable parts of the data going into the computation. Think of a CPU inside your computer. It has some very basic, very stable data baked into it, even though we don't tend to think about it this way. We don't need to - utilizing higher-level abstractions is simply more productive.

Not all computation engines are this way though. Think of your brain. It's built off of large numbers of organic, constantly changing cells. That's not to say there are no stable, or relatively stable *elements* within the context of a human brain, [there definitely are](https://en.wikipedia.org/wiki/Cortical_column). At some basic level though, the brain system is *dynamic*, plastic, quite different from a modern CPU.

Of course this could just be a question of perspective and properly chosen abstraction levels. One could argue cells are still very much based on certain stable sets of instructions available to them, based on the universal rules of physics and chemistry. This observation, while fascinating, is probably not that useful for us right now, as we still have to deal with serious limitations when it comes to.

## The practical question

Using a more static approach allows for additional optimizations. There are many possible ways to optimize for efficient computation, some easier than others. Having the model structure be static and have a fixed size in memory is a good starting point. Going further than that, we could for example try moving closer to a regular ECS approach where components and logic processing are all defined on the level of program code. This is something we're trying to avoid though, mainly due to limited scalability and long compilation times.

Dynamic model, on the other hand, opens up some new possibilities. As most of the elements like components and entities are defined from user scripts and treated more like data, it makes it faster to iterate when designing models. 

When it comes to the logic processing side of things, one of the most interesting aspects enabled by the dynamic approach is the ability to more easily create and mutate the instruction set. This opens the door to hot-reloading and switching models within the same runtime instance. It can also enable changing instruction set at runtime to accomplish things like creating agents that have the capability of learning to do things differently as they interact with the environment.

