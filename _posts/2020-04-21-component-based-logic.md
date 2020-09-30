---
layout: post
title:  "Component-based state machine logic"
description: ""
tags:
  - devlog
permalink: "/blog/component-based-state-machine-logic"
published: false
---

<meta property="og:image" content="https://images.unsplash.com/photo-1580432522609-d073f3f4b4f9?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=530&h=300&q=80"/>



One of the most important things about component-based logic is the idea of *composability*. As in "entity's behavior is *composed of* certain components". This can be thought of as a slightly different take on object-oriented approach, where instead of declaring certain basic entity objects and inheriting multiple different entities from that, we declare entity types as collections of components.

Also notice that we don't create entities as structures with certain set of variables on them, as we would in a classic object-oriented approach. Instead the variables will be added to our entities by the components as they initialize.

This may sound like a total mess. And it would be, if we didn't have any way to define common features for components, or entities too for that matter. We have this ability thanks to the reliance on the *model*.

The *model* can define a set of components for entities of some type. This is important, because it helps us build entities that are meant to represent certain *classes* of things.


Components are the main place where the logic execution happens. It doesn't have to happen through events though - we can potentially also *hook into* a component and execute custom logic on it manually. Components can hold data, but it's for internal use only.
