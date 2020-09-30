---
layout: post
title: "Entity-component architecture"
description: ""
tags:
  - devlog
  - engine
  - design
permalink: "/blog/entity-component-architecture"
published: false
---

<meta property="og:image" content="https://images.unsplash.com/photo-1517373116369-9bdb8cdc9f62?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=530&h=300&q=80"/>

Today I'll attempt to outline some of basics behind the architectural pattern on which the engine is going to be based on. Nothing in this post will be particularly groundbraking --- for the most part I'll be introducing concepts that have been well established in the industry and that are successfully used accross many different problem domains.

Keep in mind this is just an impromptu blog post. Parts of it will perhaps find their way into the documentation later on and get expanded upon, while other parts may become outdated in the near future. There's a lot of moving parts still.


## What is the entity-component pattern?

It is a way of structuring a program so that everything (or at least most things) in it's domain can be described as **entities** each with a set of attached **components**.

This approach can be described as *composition over inheritance* as it favours *composition* of behavior based on different components, instead of creating complex *inheritance* trees.

Historically speaking it's not a particularly new development, though adoption varies widely from domain to domain. Interestingly, one of the places where doing things with entities and components is probably most common is the gaming industry.


### ECS and cache efficiency 

This is an aside, as we doesn't use an ECS pattern in any real sense, but it may still be worth mentioning this point, as some of it's benefits may transfer to the solution used with this project.

Most popular versions of the entity-component pattern also include a notion of *systems* (hence the acronym *ECS*), which are a neat abstraction that basically just means *logic*. In this arrangement components and entities are just data that can be used by systems that perform computation on that data.

This approach has an interesting advantage over other patterns and most other entity-component solutions, namely it makes it easier to write programs that will perform efficiently on modern hardware. 

Without getting into too much detail, it all has to do with the fact that, once we get to sufficiently low time-scales, from the CPU point of view, fetching data from RAM is quite slow. To deal with that problem, modern CPUs have memory of their own, called *cache* memory, which is super fast, but severely limited in size. 

During execution the CPU will preload data it reckons will be needed in the near future. Some programs and programming patterns do better job than others at assisting this prefetching proces. A classic ECS system, by processing a whole array of similar components in a single swoop, is able to minimize the so called *cache misses*, and allow the program to be run more efficiently.


## Entities and components in a distributed setting

Let's recap what we've talked about so far and put it in simple terms.

For the purposes of this project, we understand the abstraction called entity to be a self-contained data structure that represents, well, an entity. When modelling we will use a notion of *entity* as our chief abstraction --- a self-contained objects quite similar to how we define separate *entities* in real life.

Important point is that entity holds within itself data that is *internally relevant* for it. For example if we were to have an entity representing a person, then that entity structure would hold within itself data about the current state of that person.

Of course this is only a general abstraction. In the real world we rarely end up with truly *self-contained* entities. Still though, this is one of the core abstractions the sim engine builds upon.

Component, then, is a modular expansion attached to an entity. This way an entity becomes a collection of components, allowing us to *compose* things like properties and behavior of any given entity.

Components are not seen as merely static data baskets, but also as elements defining actual behavior, or logic.


