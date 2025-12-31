---
title: "Where Predictability Matters: Islands of Stability in Real Systems"
slug: "02-functional-programming"
date: 2025-12-21
author: "Paulo Mendonça"
draft: false
weight: 2
description: "Referential transparency, steady state, and pragmatism in multi-paradigm languages."
tags:
  - Functional Programming
  - Software Architecture
  - Technical Diary
keywords:
  - Referential Transparency
  - Steady State
  - Immutability
  - Side Effects
  - Domain Modeling
cover: "/blog/images/02-functional-programming.jpeg"
---

I recommend reading [part one]({{< relref "01-functional-programming.en.md" >}}).

{{< translation-note >}}

# The Thesis

The natural path into part two, especially when we deal with multi-paradigm languages, is to
move past definitions and **put the thesis into practice**: it is possible to build a
**steady-state core**. My thesis is less about _becoming functional_ and more about harvesting
concrete gains by adopting practices we usually associate with functional languages, such as
immutability, explicit state modeling, and the conscious reduction of side effects. In
multi-paradigm languages this does not turn into dogma; it becomes a resource. And I propose
using those resources to buy predictability where it matters most and, better yet, where other
paradigms fail to deliver.

Is it possible to push a system toward a more "mathematical", more replaceable, more stable
core? Yes. But I do not treat this as a mandatory destination. The cognitive effort exists and
does not always pay for itself. What interests me is the point where these practices start to
create areas that are naturally easier to reason about, test, and evolve, and, in my
experience, the most fertile place for that to happen is the domain, especially when it is
rich enough to justify explicit rules, transitions, and invariants.

The concepts covered in part one serve as groundwork. In this part, I want to map where that
groundwork becomes solid road in the regions of the system where you can explore the functional
mode with less friction, not out of purism, but because stability becomes tangible there.

> The biggest mistake I see today is not ignoring Functional Programming, but believing that
> adopting it is a binary decision.

## Multi-paradigm

In the previous text I talked about Functional Programming as if I were writing from inside a
functional language. That is not the case. My practical reference comes from years writing
systems in **multi-paradigm** languages like C#, Java, and JavaScript - places where we live
between the imperative and object-oriented paradigms, and where functional ideas appear more as
**resources** than as **a contract**.

And that detail changes everything. In languages that do not force you to be "pure", it is easy
to write code that looks functional and still carry implicit dependencies underneath: a `now`,
a singleton, a global cache, a repository called in the middle of a rule, a silent mutation.
The result is that the promise of predictability does not come "for free"; it needs to be bought
with conscious decisions and, yes, with some cognitive cost.

That is why I am not arguing that we can (or should) turn an entire application functional. My
bet is more pragmatic: **there are real gains** when we adopt practices associated with the
functional world - immutability, explicit states, well-localized effects - and choose where
that pays off. And, in my experience, the most fertile place for this is the **domain**,
especially when the domain is rich enough to deserve well-defined rules, invariants, and
transitions.

## Referential transparency (RT) and steady state (SS)

I will use two terms here as tools for reasoning, not as a stamp of academic purity. The first
is **referential transparency (RT)**, which is an observable property. An expression/component
is RT when I can replace it with the value it produces without changing anything relevant in
the system's behavior.

A very practical way to feel this is to ask: *if I call this twice with the same input, do I
swear that the result is the same and that nothing "happened" in the world because of it?* If
the answer is "it depends", there is almost always some implicit dependency leaking.

The point is that, in multi-paradigm languages, RT rarely appears as a perfect "yes/no". It
appears as regions of code where RT becomes a good approximation, good enough to reduce fear,
reduce mental simulation, and increase confidence when refactoring.

The second term, **steady state (SS)**, is the structural condition that allows this property
to emerge. I consider that a piece of code enters SS when it carries no **mutable internal
state** that affects the result without appearing in the signature, when it does not depend on
**time**, **call order**, or **side effects** to "work", and when it can be understood as a
transformation: **explicit input -> explicit output**.

> **RT** is what I observe. **SS** is what I build.

In real systems, the break almost never happens "because the code is imperative", but because
it has invisible dependencies: time (`now`), randomness, IDs generated "out of nowhere",
database reads/writes, cache, queue, filesystem, network, global state (singletons, `static`,
mutable config), shared mutation (an object that "gets filled in"), and order (a method that
only works if another was called before).

None of this is a sin. The problem is when it appears mixed with domain rules, because then
the rule stops being a rule and becomes a performance staged by the environment.

This matters especially in the domain, because that is where rules, invariants, and transitions
live. In rich domains, the complexity is not in "how to call the database", but in "what can or
cannot happen". And that is exactly where SS pays off: by making inputs and states explicit, we
trade diffuse uncertainty for a narrower space of possibilities.

## Islands

In your project, "islands" will appear regardless of your architecture. It is not a promise and
not an obligation: they are stretches where you can treat rules and decisions as data
transformations, without depending all the time on the database, the network, the UI, or logs. These spaces
do not always have perfectly clear boundaries, but even a "good enough" boundary already
changes the way you think.

When you identify an island, try to protect it: keep inputs explicit, return clear outputs, and
push actions in the world (fetch/save, call an API, log) to the edge. It is not about purity; it
is about creating conditions for an **SS** to emerge and, over time, reap the fruits: fewer
surprises, less fear of touching things, more direct tests, and steadier evolution.

## Conclusion

This text is a report; everything I described here came from a long time testing various
possibilities. I invite you to start testing: go slowly, explore each theme individually, and
keep going whenever you feel comfortable, but only when you begin to see value. Remember:
if form has no function, it has no value.

Over time you will begin to experience fewer surprises, less fear of touching things, more
direct tests, and a better pace of evolution. **Today, when I look at a system, I do not ask if
it is functional or object-oriented. I ask where predictability matters most, and how much I am
willing to pay for it.**

See you, and thanks for all the fish!
