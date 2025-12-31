---
title: "Explicit State Modeling: From Value to Time"
slug: "03-modelagem-explicita-de-estados"
date: 2025-12-28
author: "Paulo Mendonça"
draft: false
description: "A reflection on how domains gain semantics, protection, and expressiveness as states stop being implicit."
weight: 3
tags:
  - Explicit State Modeling
  - States as Types
keywords:
  - Primitives
  - Value Objects
  - Marker Types
  - Phantom Types
  - Typestate
cover: "/blog/images/03-cover.jpeg"
---

{{< translation-note >}}

# Introduction

In parts one and two I tried to tell a story. At times I failed and the text became a mix of
what happened to me and what I should do. That gave it an inelegant, almost tutorial-like
tone. I will try to correct that here, keep the narrative cleaner, and take the opportunity to
thank everyone for the feedback.

While I was venturing into the still little-explored paths of explicit state modeling, I
started to ask why this approach is so invisible in multi-paradigm languages like C#, Java,
TypeScript, and the like. The answer, in the end, was exactly what one would expect. These
ecosystems grew under an imperative mindset oriented around mutable data, where state is
usually represented by flags, enums, and scattered conditionals. Making states explicit
requires the opposite: anticipate possible states, forbid invalid combinations, and model
transitions. This increases the initial effort and reduces the feeling of short-term speed. In
deadline-driven environments, that friction is often perceived as bureaucracy - even when it
reduces serious errors in the medium and long term.

Besides that, even though these languages allow explicit state modeling, there are almost no
ergonomic incentives for it. The type system is powerful, but optional and often bypassed. It
is easy to fall back to null, bool, string, or even an any and move on. Because state errors
appear late, the real cost stays invisible to whoever writes the initial code. The result is a
vicious cycle: implicit states seem simpler, become the default, and any attempt to make them
explicit sounds "too complex". In the end, this approach merely exposes a complexity that was
always there - hidden gaps, postponed decisions, things we leave for tomorrow's self to
resolve.

## Building blocks

I ended up meeting these actors organically; some I understood better while using `rust` and
venturing into `elixir`. Even though some are already well known, I think they are still worth
mentioning. And here is where the text might start to feel a bit like a tutorial. Here I am
again, slipping in the narrative. I will present who they are and what they are, but it is not
time yet to talk about the "how" and instead about the "way of thinking" around this theme.

### Primitives

Ground zero. `int`, `string`, `Guid`, `decimal`... They have no semantics, are cheap, dangerous,
and inevitable at the edges. They do not belong to the domain - but, paradoxically, the domain
almost always starts with them.

Primitives are dangerous because they carry value but not meaning. An `int` does not say what
it represents; a `string` accepts anything; a `Guid` is just an identifier without identity.
The compiler cannot distinguish a `UserId` from an `OrderId`, an `Age` from a `Quantity`, a
`Price` from a `Balance`. When the domain is expressed directly with primitives, you transfer
semantic responsibility from the **type system** to **human memory**, comments, conventions,
and discipline. And that does not scale. The error does not show up where the code is written;
it shows up later in wrong integrations, invalid states, and rules silently violated.

They are seductive. They are cheap, quick to write, universal, and therefore become the
standard path, especially in multi-paradigm languages. But that low cost is an illusion; what
is saved in modeling is paid for with scattered validations, defensive `if`s, redundant tests,
and hard-to-trace bugs. That is why primitives are inevitable at the edges (I/O,
serialization, database, and network), but toxic at the core. They do not belong to the domain
because the domain is semantics, rules, intention. And all of that begins precisely when we
abandon **raw primitives** and start naming, constraining, and protecting the meaning of what
the system truly is.

### Value Objects

Value Objects carry a minimal semantic identity. Here we encapsulate validations, remove basic
ambiguities, and create equality by value. **Here happens the first real domain leap.**

VOs represent the **first real domain leap** because they are the moment when the system stops
manipulating generic values and starts manipulating explicit meanings. An `Email` stops being
just a string and starts carrying rules of validity, format, and intention. A `Money` stops
being a `decimal` and starts enforcing currency, precision, rounding, and allowed operations.
A `UserId` stops being a technical identifier and becomes an identity recognized by the
domain. Here, semantics leave the developer's head and enter the code, turning trivial errors
into **construction impossibilities**.

It is also in VOs that a fundamental change in behavior is established: equality by value,
immutability, and invariant encapsulation. This drastically reduces invalid states, eliminates
duplicated validations, and makes the code more readable, because the type starts to
communicate intent. Still, it is a minimal leap.

VOs do not model processes or transitions, but they create a foundation where the domain can
exist without ambiguities. Without them, any attempt at advanced modeling will be built on
quicksand.

### Marker Types

They serve to perform **conceptual qualification**. They do not change data; they change the
meaning by creating semantic categories. Here the domain begins to **say things about itself**.

Marker Types perform **qualification** because they introduce a layer of meaning without
adding data or behavior. They exist only to classify, label, and differentiate something that,
structurally, could be identical. A `User` object still has the same fields, but a
`User<Authenticated>` is not conceptually the same as a `User<Anonymous>`. The marker does not
transform the value; it transforms what the value represents within the domain. That
distinction, made in the type system, allows certain operations to become illegal by
definition, without `if`, without `bool`, without defensive validations.

When I say that from here on the domain starts to **say things about itself**, I mean semantic
self-description. The code stops depending on comments, conventions, or external documentation
to explain under which conditions something stands. The type itself communicates _this has
already been validated_, _this has already been authenticated_. The domain starts to carry
claims verifiable at compile time about its state. It is no longer the programmer saying
"trust me, this is valid!", but the compiler saying it.

At this point the domain stops being just a set of data with implicit rules and starts to
behave like a language that describes itself.

### Phantom Types

They are relational invariants; here I encode preconditions in the type, eliminate invalid
sequences, and prevent wrong uses. In short: the domain starts to **defend itself**.

Phantom Types make the domain defend itself because they shift precondition checking from the
**runtime** to the type system. They do not exist at runtime, do not carry data, but carry
relational constraints such as: _this value can only be used after a certain condition is
satisfied_, _it only makes sense in combination with another specific type_, _it can only go
through certain operations_. The type stops representing only "something" and starts
representing something that can be used under specific conditions. Invalid sequences are
simply not expressible and there is no way to "skip steps", because the compiler will not
allow it.

This is where the domain starts to **defend itself** because it no longer trusts the
developer's discipline or late validations. Errors stop being possibilities and become
structural impossibilities. An unvalidated resource does not reach functions that require
validation and a context identifier cannot be passed to another. The domain becomes a
**system of living constraints**, where wrong uses are not tolerated. At this point, the code
stops "hoping people will do it right" and starts enforcing the **right by construction**.

### Typestate

They are responsible for explicit temporal rules. Here the domain expresses **order**, valid
transitions, and tightens the fence against illegal states. This is the highest point of
expressiveness I have managed to see in multi-paradigm languages.

Typestates make the domain's temporal rules explicit by modeling not only which states exist,
but in what order they can occur. Each relevant state becomes a distinct type, and each valid
operation is, in practice, a typed transcription. It consumes one state and produces another.
Time, usually implicit in comments, mental flows, or external documentation, becomes encoded in
the function signatures themselves. It is no longer possible to call something "out of turn",
because the compiler requires the object to be in the correct state before the operation can
exist as a possibility.

That is why this is the highest point of expressiveness accessible in multi-paradigm languages
that I have been able to find. The domain not only carries meaning (VOs), nor only qualifies
itself conceptually (Markers), nor only protects itself from misuse (Phantom Types). With
Typestates, it starts to express behavior over time, turning flow into type and order into
structure. The code stops being a sequence of defensive instructions and becomes a formal
description of what can and what can never happen.

## Conclusion

Maybe the real reason explicit state modeling is so rare is not technical, but cultural. It
forces us to face something uncomfortable: complexity does not arise because we write "bad
code", but because the domain is complex. And when we make that explicit, we lose the comfort
of vague abstractions and postponed decisions. Primitives, flags, and ifs are not neutral
choices; they are ways of pushing responsibility into the future. Modeling states explicitly
is doing the opposite; it is taking on now the cognitive cost we normally leave for the system
to pay later. Perhaps that is why this approach feels so unsettling. It does not promise
immediate speed; it promises only something harder to sell: fewer surprises when it is already
too late.
