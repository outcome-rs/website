---
layout: post
title:  "Readable addresses and question of string indexing"
description: ""
tags:
  - devlog
  - engine
  - design
permalink: "/blog/readable-addresses-and-string-indexing"
published: true
---

<meta property="og:image" content="https://images.unsplash.com/photo-1529066496421-d8fb2dca38a6?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=691&q=80"/>

Today I sit down to write down a few thoughts I had regarding the problem of *addressing* and what is still to be done. I also tried to wrangle with the question of implementing proper storage indexing for objects and variables.

## Readable addressing

I've partially solved the problem of readability and accessibility of references to variables and objects by introducing the `Address` abstraction. Being composed of multiple parts, an `Address` reflects the overall architectural layout involving entities, their components, and variables.

A full `Address` serves as an unique identifier for any variable within the simulation. But non-full addresses are also used, and there needs to be a well defined way for using those as well.

Such "partial addresses" could also be about more than just the end-point variables. The same format could potentially be used for referencing simulation objects, or even just specifying ones that perhaps don't yet exist.

I think a notion of **signature** is what I'm looking for here. Here's a quick example:
```
# entity type signature
:ent_type:*
# entity signature
:ent_type:ent_id
# component type signature
:ent_type:*:comp_type:*
# component signature
:ent_type:*:comp_type:comp_id
# entity-local component signature
comp_type:comp_id
```

Note the use of the star `*` sign. It brings the idea of a *wildcard* to mind, which is good, since that's what we mean when we specify those signatures - we mean "here goes anything".

The same format could also be used to specify *"matches"* that could be expanded based on the simulation state and existence, or lack thereof, of certain objects.

For example a query could be sent through the `Address` interface to expand the following wildcard signature: `:ent_type:*`, in which case the response would include "filled-in" signatures of all existing entities of type `ent_type`. This should work for multiple wildcards as well.


## Is string indexing bad?

`Address` interface deals with human readable strings. That's what it does. Now, translation between strings and more machine readable constructs like integers for more efficient storage and execution could be implemented, but how useful would that be exactly? And how much complexity would it add to the existing system?

Current `outcome` implementation relies heavily on string indexed maps. Noted, it's not using the default heap-allocated `String` structure shipped with Rust, instead utilizing a fixed-length array-based solution. But even with that, these strings are still larger than your average 64 bit integer.

Attempting to put the RAM overhead from indexes under control, the default length of an `IndexString` is set at only 10 characters. That's quite short, but still usable if you manage to make those names shorter. In terms of bytes used, 10 characters long array-based string takes 11 bytes, about 37% larger than a 64 bit integer.

When compiling the library you can enable the `long_indexstring` feature which will set the length of `IndexString` to a whopping 23 characters, or 24 bytes. At this point a single `IndexString` is 3 times the size of a 64 bit integer. We're definitely not going to have so many entities, or anything for that matter, to use all the space there. So in terms of *compression* we're not doing so good.

But what exactly could be done here? Remember that we still want to optimize for **quick access of all the simulation data** using string based addresses. We can't just get rid of that information.

One idea would be to set up an intermediate translation mapping. While this approach wouldn't help us minimize RAM usage (it would actually increase it, unless we decided to only set selected data as externally accessible), it could speed up execution, since smaller indexes usually translate into faster lookups and insertions.

But it wouldn't be as easy as simply creating a mapping at startup and calling it a day. Things like entities can be spawned at runtime, so we need to handle translation at runtime. On top of that, there is another problem. How exactly would this assigning of integer names work in a distributed setting? You would very likely need the central authority orchestrating the whole process. Or perhaps introduce some clever solution like assigning each cluster node a set of integer id's it can use, to deal with name clashes. But still, how would you get data from one node to another, without a global reference table? It sure sounds complicated... I mean it's doable, but would introduce additional layer of complexity which is not something I'm looking for right now.

Is string indexing all bad? Given the requirements, I don't think so. It's far from great, but still relatively simple. Let's call it *good enough* for now.
