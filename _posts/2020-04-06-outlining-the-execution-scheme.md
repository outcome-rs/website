---
layout: post
title:  "Outlining the execution scheme"
description: ""
tags:
  - devlog
  - engine
  - design
permalink: "/blog/outlining-the-execution-scheme"
published: true
---

<meta property="og:image" content="https://images.unsplash.com/photo-1568057373106-63057e421d1c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=534&h=300&q=80"/>


So we already know we want to have a way of processing logic on the level of entities, which will possibly allow some amount of parallelization. How do we proceed from there?

We need to define synchronization mechanism that will give structure to the whole processing system. And that's what I will attempt to outline in this post. 


## Global synchronization and a notion of *step*

Global, that is applicable-to-all-entities-at-all-times, synchronization is very useful for many kinds of simulations. It allows us to keep the whole system coherent as it's being run, minimizing risk of strange unsuspected and/or undesired effects and errors. 

### To synchronize or not to synchronize

While often useful, global synchronization is not strictly necessary for every situation. In fact there are cases where it might be better not to use it.

Imagine you were building a massive multiplayer online game and chose to use `outcome` to hold a large number of entities accross a cluster of machines. Your game logic would run your custom code deployed as multiple external services to interact with the entity data.

Now, the game world would not need to be kept in perfect sync every frame. That would slow things down. You want to get the data as fast as possible, tens of frames per second. Frame-to-frame coherency of the world is not that important here, since players are not going to notice it anyway --- someone playing in one area simulated by one game-engine service doesn't need to experience perfect synchronization with another user on the other end of the map simulated by another service.


## Defining execution phases

Let's focus on the runtime-level logic processing for a while.

Within a single *step*, the engine's built-in logic executor shall operate with the notion of distinct execution phases, that will allow for synchronized processing in a distributed setting. Separation of *internal* (or *local*) and *external* paradigms when it comes to entities is important here.

For each simulation processing step there shall be three execution phases:
1. **preliminary** or **pre** phase, where entities exchange data between each other (this phase could be gotten rid of in the name of performance)
2. **local** phase, where entities are processed in separation
3. **central** or **post** phase, where instructions requiring global state, like adding new entities or changing the model, can be executed; entities can exchange information between each other here as well

While not exactly a novel or particularily brilliant idea in itself, this phase separation is useful enough to become the backbone for all processing performed on the runtime level.

### Local and external commands

Phase-separation scheme requires us to also differentiate between different kinds of commands. Current implementation defines three basic types of commands:
- **`Command`** meant for execution during the **local** phase, model contains a list of these for each component
- **`ExtCommand`** meant for execution during either **pre** or **post** phase, model only contains a list of these for **pre** phase, for **post** phase a list is assembled during the **local** phase
- **`CentralExtCommand`** executed during **post** phase, list of these is assembled during the **local** phase

As you can tell, most of the execution is organized around the local `Command`. This makes sense, as we want all the important logic to be handled during the *isolated execution* of an entity in it's own thread. Since that logic includes flow control and the like, we can only know the contents of **post** phase after we're done executing the **local** phase.


