---
layout: page
title: The Outcome Project - Open Source Distributed Simulation Software
nopostfix: true
---

<div class="page" markdown="1">

<div style="font-size: 24px" markdown="1">
Creating an open source **distributed simulation** solution that's fast, flexible and easy to use.

Aiming at enabling **open, collaborative work on complex models** --- from multiplayer game worlds and domain specific experiments to organic city systems and full-scale economies.


</div>
<br>


## Why is this needed?

Because we are currently facing a lack of open, easy to use tools enabling modeling and simulation of complex emergent systems in a scalable way.

Developing such tools, and making sure they will be accessible to the average person, has enormous potential to change how we approach complex problems --- both as individuals and as a society. 

It can also enable people to create cool new experiences, games and art that would be impossible without this kind of technology.

<br>

## How does it work?

It combines the idea of basic composable entity-component architecture with notions of distributed processing and storage. Result is a system that can handle millions of entities running concurrently on multiple different machines, no matter whether these are laptops, phones or dedicated clusters.  

Specialized *service processes*, anything from physics engines to AI solvers, can work on the entities' data either locally or remotely, building up a coherent, unsharded world.

On the runtime level there is support for execution of synchronized, state-machine based scripted logic, which is easy to write and doesn't require any prior programming experience.

On a higher-level of abstraction, user-friendly notions of *modules*, *scenarios* and *snapshots* are used. Low-level phenomena like message-passing and load balancing are abstracted away. For many use-cases, running simulations is as easy as downloading an executable and modifying manifest files.

<br>

## How to get started?

[<b><u>The book</u></b>](https://book.theoutcomeproject.com) contains everything you need to get started. It will not only teach you about the underlying concepts and implementation details, but also guide you through installation and writing your first simulation models. This way you should be able to follow along even if some of the presented concepts are still confusing or unclear.


</div>
