---
title: "Arc Foundation: States, Types, and Time"
slug: "fundacao-arco-estados-como-tipo"
date: 2025-12-31
author: "Paulo Mendonça"
draft: false
description: "Framing text for the explicit state modeling arc: why implicit states fail and how types carry meaning."
weight: -1000

tags:
  - Functional Programming
  - Domain Modeling
  - Explicit State Modeling
  - States as Types
  - Software Architecture

keywords:
  - Implicit States
  - Explicit States
  - States as Types
  - Explicit State Modeling
  - Type System
  - Semantics in the Type
  - Typestate
  - Phantom Types
  - Marker Types
  - Value Objects
  - Predictability
  - Referential Transparency
cover: "/blog/images/00-cover.jpeg"
---

{{< translation-note >}}

# This arc was never about Functional Programming

Most of the most serious bugs I have seen over more than twenty years working with software did
not come from wrong algorithms, but from misunderstanding concepts I did not yet know how to
name, ideas that for a long time felt too abstract, almost esoteric, and therefore very easy
to ignore.

The curious thing is that the problems almost always had the same apparent culprits. We spoke
of **software entropy**, of **bad design decisions**, of **unresolved legacy**. And those
explanations made sense, but only later. The fact is that in the beginning, no one decides to
build a complex system. No one wakes up wanting to make bad decisions. Those labels only show
up when the damage is already done.

What took me a while to realize is that all those "culprits" were, in fact, side effects of
something more fundamental: too many states being carried without language, without structure,
and without protection. The problem was not that decisions were bad from the start; it was that
we did not know what we were deciding.

## The real problem: invisible states

In many systems, state does not live in a single place. It emerges from the combination of
flags, mutable properties, optional values, and the order in which methods were called. To
understand "what state we are in," you need to reconstruct a mental timeline.

This model works while the system is small, the team is the same, and the context is still
fresh. Over time, it starts to fail quietly. Knowledge stops living in the code and starts
living in people's heads. The domain still exists, but its semantics dissolve into defensive
conditionals, scattered validations, and implicit conventions.

Flags, enums, and ifs are seductive because they give speed in the short term. But they charge
high interest later. They push responsibility into the future, to the next developer, to the
next refactor, almost always to when it is already too late.

**Why is this so common in multi-paradigm languages?**

Languages like C#, Java, or TypeScript do not prevent explicit state modeling. They allow it.
The problem is that they do not encourage it.

The type system is powerful, but optional. It is always possible to bypass a restriction with
a null, a bool, a string, an any. Primitives are cheap, universal, and quick to use and that is
exactly why they become the default. The real cost shows up later, when the domain starts to
demand guarantees that were never formalized.

This creates a vicious cycle: implicit states seem simpler, become the norm, and any attempt to
make them explicit sounds like unnecessary complexity. In the end, we are not creating new
complexity. We are only exposing a complexity that was always there, but that we preferred to
ignore.

## The turning point: semantics in the type system

At some point it became clear to me that the problem was not control flow. It was lost
semantics.

When state is not in the type, it is scattered across the code. When validity is not in the
type, it depends on discipline. When order is not in the type, it depends on memory. Everything
the compiler could verify becomes a human responsibility, and that does not scale.

The type system stops being just a technical detail and starts to function as external memory
for the domain. It does not carry only data; it carries meaning, constraints, and context. It
says what something is, under what conditions it can exist, and what can happen next.

This is where this arc takes shape.

## The conceptual ladder of this arc

Across these texts, I explored a ladder of expressiveness that did not emerge as an isolated
technique, but as a way of thinking:

**Primitives** -> **Value Objects** -> **Marker Types** -> **Phantom Types** -> **Typestate**

This sequence does not represent "levels of sophistication", nor a mandatory path. It
represents progressive ways to make states explicit, move guarantees from runtime to the type
system, and reduce the space of possible states.

Each step answers a different question:

- What does this mean?
- What condition is it in?
- What can or cannot be done now?
- In what order can things happen?

Not as a technique. As the language of the domain.

## Where Functional Programming fits in -- and where it does not

Functional Programming appears in this arc as a tool, not a dogma. Concepts such as
immutability, referential transparency, and the conscious reduction of side effects help create
regions of code that are more predictable, more stable, and easier to reason about.

In multi-paradigm languages, this does not happen in a total way. It happens locally. In
islands. Especially in the domain, where rules, invariants, and transitions matter more than
integration with the database, network, or UI.

This is not about "becoming functional". It is about choosing where to pay the price of
predictability and where to accept imperfection. The goal was never purity. It was confidence
when reading, modifying, and evolving.

## Pay now or pay later

Modeling states explicitly is uncomfortable because it anticipates decisions. It forces us to
assume now a cognitive cost that we normally push into the future. Primitives, flags, and ifs
are not neutral choices; they are ways of postponing responsibility.

This arc is an attempt to do the opposite: accept that the domain is complex and treat it
honestly. Not to make it simple, but to make it understandable. Because understandable systems
fail less -- and, when they fail, they fail in less surprising ways.

If this text serves any purpose, let it be as an entry point. Not to Functional Programming,
but to a more fundamental question:

> _"Where is the state of my system really being decided -- and who is paying for it later?"_

## Conclusion

This arc is not the result of a few days of research or a passing enthusiasm for a new
technique. It is the distillation of an entire year, **2025**, dedicated to observing, testing,
failing, refactoring, and reconsidering choices in real systems.

Nothing here emerged in a vacuum. Each idea was supported by more than two decades working as a
programmer in production environments, dealing with deadlines, legacy, teams, pressures, and
real consequences. If there is any conviction in these texts, it does not come from isolated
theory, but from repetition, contrast, and enough time for patterns to become visible.
