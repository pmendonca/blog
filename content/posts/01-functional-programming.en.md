---
title: "Functional Programming: Technical Diary – Part 1"
slug: "01-functional-programming"
date: 2025-12-13
author: "Paulo Mendonça"
draft: false
weight: 1
description: "Practical reflections on how Functional Programming concepts change the way we work with real systems."
tags: ["FP", "Functional Programming", "Software Architecture"]
keywords: ["Immutability", "Algebraic Types", "Side Effects", "Explicit Modeling"]
cover: "/blog/images/01-functional-programming.jpg"
---

[Ler em português](/blog/pt-br/posts/01-functional-programming/)

Dear reader, this is not a didactic blog. It does not aim to be a tutorial, a manifesto, or a definitive guide. It is a **technical diary**, written for myself, about what happened when **Functional Programming** concepts blended into real code, in a real project with deadlines, legacy, teams, and real-world systems.

This journey began like many others, driven by curiosity. I will describe how, very quickly, the results exceeded any expectation. Common fears faded away: the constant anxiety of changing code and the strong dependence on historical context. What follows is a diary that I now choose to share with you.

## **Immutability** as a central theme

My first entry point was understanding the idea behind immutability. Not as an abstract concept, but as a conscious attempt to reduce the number of things that could change without me noticing. Some language decisions that I now recognize merely as tools emerged naturally from this discomfort and deserve a text of their own. Here, what matters is the effect: **fewer implicit states**, **fewer surprises**, and more **confidence when reading and modifying code**. Many concerns were mitigated or became irrelevant.

### **Copying** is better than modifying

The first implication of making something immutable is the need to create a new state for an object whenever a change is required. And although this may look like waste at first glance, it is in fact a strategy. When we stop sharing mutable objects across parts of the system, we stop relying on “discipline” and start relying on a rigid rule where _each step receives its own value_. This reduces the space of possible states by eliminating aliasing (when two references point to the same thing) and, as a result, cuts off an entire class of bugs caused by mutations outside the field of view — **cascading effects**, **race conditions**, and that familiar kind of “it works until it doesn’t”.

What feels counterintuitive is that this approach **can be more performant**, because the real cost in large systems is rarely **copying bytes** — it is **coordination**. Locking, synchronizing, resource contention, cache invalidation, waiting on I/O, serializing access, debugging shared state — **that is what’s expensive**. In most cases, creating a new value (or a new instance) is cheaper than paying the toll required to guarantee that shared state will not be corrupted. And even when copying has a cost, it is predictable, local, and easy to measure; the cost of shared state, on the other hand, tends to be intermittent, emergent, and difficult to attribute.

In the end, **“copies”** here are not a functional indulgence, but a way to trade **global complexity** (coordination) for **local work** (value derivation). Repeated hundreds of times in real code, this turns into safety — and often into performance.

### Explicit **state modeling**

Another area where I felt a deep shift was when I started to **model states explicitly**, instead of leaving them implicit in object behavior. In many traditional systems, the real state of something does not live in a single place; it emerges from a combination of flags, mutable properties, and the order in which methods were called. To understand “what state we are in,” one must mentally reconstruct a timeline.

When state becomes explicit, that reconstruction is no longer necessary.

Explicit state modeling means accepting that a system **is not in any state at any time**. It is in **one of a few allowed states**, well defined, known, and named. More importantly, certain transitions simply **do not exist**. They are not forbidden by convention, documentation, or tests — **they are not representable in code at all**.

The effect is immediate. Questions like _“can this happen?”_ stop being answered with _“I think so”_ or _“it depends on the flow”_. The answer becomes structural: either the state allows that operation, or it doesn’t compile, doesn’t fit, doesn’t make sense.

This kind of modeling changes the role of code. It stops being just a sequence of instructions and becomes a **map of what is possible**. Invalid states do not need to be handled; they do not exist. Unlikely transitions do not need to be defended against; they are not modeled.

The gain here is not only algorithmic. It is cognitive. Reading the code becomes an exercise in understanding **where we are**, not deducing **how we got here**. And this dramatically reduces mental load, especially in systems that evolve over time.

Today I realize that many bugs I once attributed to “domain complexity” were, in fact, consequences of **too many implicit states**. By making them explicit, the domain did not become simpler — it became honest. And honest systems are easier to maintain.

### Conscious reduction of side effects

Over time, I realized that immutability and copying alone do not solve everything. They help a lot, but the real gain only appears when **side effects** start being treated as something that must be explicitly decided, not as an unavoidable detail of code.

For a long time, side effects were simply “part of the job”: writing to a database, calling an API, logging something, publishing an event. The problem is that when these things are mixed with domain logic, the code stops explaining _what it does_ and starts explaining _what happens when it runs_. The difference seems subtle, but it completely changes how we reason about systems.

Reducing side effects does not mean eliminating them. Real systems need to interact with the external world. What changed for me was the awareness of **where they happen** and **when they happen**. When most of the code describes value transformations rather than interactions, system behavior becomes predictable by inspection, not by mental simulation.

The immediate effect is that errors stop spreading. A misbehaving side effect becomes a localized problem instead of something that contaminates the entire codebase. Testing no longer requires elaborate setups, complex mocks, or “artificial” states. And perhaps most importantly, reading the code once again becomes an exercise in understanding, not suspicion.

