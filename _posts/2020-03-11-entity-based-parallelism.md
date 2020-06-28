---
layout: post
title:  "Entity-based parallelism"
description: ""
tags:
  - devlog
  - engine
  - design
permalink: "/blog/entity-based-parallelism"
published: true
---

<meta property="og:image" content="https://images.unsplash.com/photo-1573122807824-55c9d19e2db5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=534&h=300&q=80"/>


As explained in the previous post, the engine is based on a conceptual pattern involving entities composed of components. It's a good high-level approach, but it still needs to be expanded upon. We still haven't defined how logic processing should be handled.

In this post I'll try to unpack the idea of entity-based parallelism, both in terms of *local* execution performed by the runtime itself, as well as in terms of external processes (for the purposes of this project often called *services*) querying and mutating entity data living in the cluster.


## Entities as externally accessible data

One of the interfaces provided by the engine involves allowing external processes running somewhere in the network to access and mutate data stored on the cluster. In terms of work parallelization this approach is fairly flexible --- as far as the engine is concerned, the work can be performed with arbitrary levels of "paralellness". The engine itself only hands out the data it's asked for. 

Efficiency of work parallelization here will differ based on many things, like actual load and amount of data transfered, cluster topology, approach to load balancing and spreading of enitities across machines, etc. It will also depend on the type of work performed by the external processes.

For example having one of them continuously compute some aspect of global state based on all the entities would be rather slow with this sort of system. Dividing up the work, so that it can be done by multiple separate processes at once, eliminating global points of congestion, is crucial here.

We won't dive into details of all this here, as we haven't yet discussed any of the networking aspects. Put a pin in that for now.


## Local entity-based logic processing

With all entities being *self-contained* in terms of data ownership, we are presented with a clear opportunity --- namely a possibility for parallel execution on the runtime level, on the level of the process/machine where the given entity lives.

This is true for *local* logic that only involves data contained within a single entity; *local* logic performed on the level of one entity should also be able to query data from other entities, which introduces a lot of complexity and need for some sort of synchronization.

We'll discuss this in more detail in the future, but for now let's just say that this runtime-level processing requires 


### Example

In very simple terms, what it means is that if we had two entities, and we were on a machine that had a CPU with two cores, we could send each of our entities to a separate core and execute their *local* logic there. This way we could utilize multiple cores availble on the CPU.

We don't have to stop on the level of a single machine --- we could just as well have our entities live on separate machines and they would be able to perform *local* logic execution just as well.

Still, however, we need to solve the problem of *coupling* the various entities, so that they are able to share data between each other, either when they live on the same machine on are spread throughout a cluster.


## Handling inter-entity information flow

So we want to utilize the fact that we can perform some *entity-local* logic that only involves entity's internal data, but we also want to be able to allow information to flow between entities. 

Sounds like we need to give this process some sort of structure. 






