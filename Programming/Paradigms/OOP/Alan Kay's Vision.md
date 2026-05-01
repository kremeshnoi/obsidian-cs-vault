#programming #paradigms

# Alan Kay's Vision

> *"I'm sorry that I long ago coined the term 'objects' for this topic because it gets many people to focus on the lesser idea. The big idea is messaging."*
> — Alan Kay, 1998 (quoted in [Eric Elliott, Medium, 2019](https://medium.com/javascript-scene/the-forgotten-history-of-oop-88d71b9b2d9f))

## The Standard Answer Nobody Questions

Ask any developer what OOP is, and you'll hear the same four words: **Encapsulation, Inheritance, Polymorphism, Abstraction** — the so-called "four pillars." These concepts are treated as if they were handed down on stone tablets alongside the term itself.

They weren't. The four pillars are a pedagogical artifact — a simplification that became canon through an accident of industrial and academic history, disconnected from the original intent of the person who coined "object-oriented programming."

## Alan Kay's Actual Definition

Alan Kay coined the term "object-oriented" around 1966–1967 while at the University of Utah, influenced by Sketchpad, Simula, the ARPAnet design, and his background in biology and mathematics.

In a [2003 email exchange with Stefan Ram](https://www.purl.org/stefan_ram/pub/doc_kay_oop_en) — the most cited primary source on this topic — Kay wrote:

> *"OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things. It can be done in Smalltalk and in LISP."*

His three essential ingredients, as reconstructed from this and other statements:

| Kay's OOP | Modern "Four Pillars" |
|---|---|
| **Messaging** — autonomous objects communicate only via messages | Inheritance |
| **Local state retention & protection** — state is private, objects own it | Encapsulation (partially) |
| **Extreme late binding** — everything resolved at runtime | Polymorphism (partially) |

> [!warning] What's missing from the modern list
> Notably, **inheritance and class hierarchies are absent** from Kay's definition. He considered focus on subclassing a distraction from the real ideas.

His biological metaphor was explicit: objects should behave like cells in an organism — each self-contained, communicating only through chemical signals, with no direct access to another cell's internals. This is much closer to how **Erlang processes** or **actors** work than how Java classes work.

## The Historical Divergence: Two Roads from Simula

The confusion starts because OOP did not evolve linearly. Two distinct paths emerged from **Simula 67** (Ole-Johan Dahl and Kristen Nygaard, 1967):

### Path A — Kay's Bio/Net Route (Smalltalk)
- Objects as autonomous processes
- Communication via messages as the primary abstraction
- Late binding everywhere
- State encapsulated, never shared directly
- Led to Smalltalk, Self, Erlang's actor model

### Path B — Abstract Data Types Route (C++, Java)
- Objects as structured data containers
- Emphasis on type hierarchies and code reuse via inheritance
- Classes as blueprints
- Led to C++ (Stroustrup, early 1980s), then Java

Stroustrup explicitly drew from Simula, not Smalltalk. When C++ popularized OOP in industry, it brought Path B's assumptions with it — and those assumptions became the default understanding of what OOP *is*.

> [!info] Two paradigms, one name
> Path A and Path B are fundamentally different philosophies that share the label "OOP." Most mainstream discourse treats Path B as the entirety of OOP, erasing Path A almost completely.

## How the "Four Pillars" Got Canonized

### Step 1: Grady Booch (early 1990s)

In *Object-Oriented Analysis and Design with Applications* (Booch, 1994), OOP was described around four characteristics, but they were: **Abstraction, Encapsulation, Modularity, Hierarchy** — not quite the list we know today. Booch's framing was still conceptually richer than what followed.

### Step 2: Java and University Curriculum (mid–late 1990s)

When Java launched (1995) and became the dominant teaching language, thousands of introductory textbooks were written to accompany it. Java's design — mandatory classes, single inheritance, explicit interfaces — made the APIE list (Abstraction, Polymorphism, Inheritance, Encapsulation) a natural fit. It described the *language features*, not the paradigm's philosophy.

As Kay himself said in the ACM Queue interview (2004–2005):

> *"I fear — as far as I can tell — that most undergraduate degrees in computer science these days are basically Java vocational training."*
> — [ACM Queue, Vol. 2, No. 9, Dec/Jan 2004–2005](https://queue.acm.org/detail.cfm?id=1039523)

### Step 3: Textbooks → Interview Culture → Dogma

The four pillars spread from textbooks into job interview prep culture, then into interview questions themselves, creating a self-reinforcing loop. By the 2000s, it was conventional wisdom. The pedagogical shortcut had eaten the underlying idea.

## What Was Actually Lost

The critical concept that vanished from mainstream OOP discourse is **messaging as the primary unit of program composition** — not method calls, not data sharing, but bounded, autonomous, asynchronous communication between components that hide all their internals.

This has concrete design implications:

- **Method calls** (Java/C++) are synchronous and tightly coupled — the caller knows the callee's interface in detail.
- **True messaging** (Kay's vision) implies no assumption about how the receiver handles it — the receiver could ignore it, queue it, delegate it, transform it.

This distinction maps directly onto modern distributed systems thinking. The internet itself — which Kay worked adjacent to during ARPAnet — is closer to his OOP vision than any class hierarchy.

> [!tip] Kay's OOP is alive — just not called OOP
> Paradigms like **the Actor Model** (Erlang, Akka), **microservices**, and even **functional reactive programming** are often closer to Kay's original intent than typical Java OOP. The ideas survived; the label didn't follow them.

## The Nuance: Kay Isn't Entirely Right Either

This is worth noting for intellectual honesty. Hillel Wayne's [2019 analysis](https://www.hillelwayne.com/post/alan-kay/) makes a strong case that:

- Objects, classes, and instances came from **Simula**, not from Kay.
- The 1981 Byte magazine issue that popularized Smalltalk explicitly credited Simula for "the fundamental idea of objects, messages, and classes."
- Kay's own early writings (Smalltalk-72 manual) do discuss classes centrally.
- Kay's messaging-first narrative hardened *retrospectively*, years after the fact.

The ACM gave the Turing Award for OOP to **Dahl and Nygaard** (Simula's authors), not to Kay — a deliberate historical attribution.

So the full picture is: OOP is a synthesis. Kay's messaging emphasis is a genuinely important and undervalued thread, but it isn't the *only* valid reading of OOP's origins. The problem isn't that people teach the "wrong" OOP — it's that they teach *one implementation's features* as if they were the paradigm's essence, without acknowledging the conceptual richness underneath.

## Summary

```
Alan Kay coined "OOP" (1967)
  └── Messaging, late binding, state isolation
      └── Smalltalk (Xerox PARC, 1970s)
          └── Influential but marginalized commercially

Simula 67 (Dahl & Nygaard)
  └── Classes, inheritance, type hierarchies
      └── C++ (Stroustrup, 1983)
          └── Java (Sun, 1995)
              └── University curricula
                  └── Textbooks: "four pillars"
                      └── Interview culture
                          └── Dogma
```

The four pillars — Encapsulation, Inheritance, Polymorphism, Abstraction — describe **Java's feature set**, not the philosophical core of object-oriented design. They are useful implementation concepts, but mistaking them for the definition of OOP is like mistaking a car's gear system for the concept of transportation.

## Primary References

- **Alan Kay's 2003 email to Stefan Ram** (canonical primary source on Kay's OOP definition):
  https://www.purl.org/stefan_ram/pub/doc_kay_oop_en

- **Alan Kay — Wikiquote** (1997 OOPSLA Keynote and other talks):
  https://en.wikiquote.org/wiki/Alan_Kay

- **"The Computer Revolution Hasn't Happened Yet"** — Alan Kay, 1997 OOPSLA Keynote (video):
  https://www.youtube.com/watch?v=oKg1hTOQXoY

- **ACM Queue: A Conversation with Alan Kay** (Vol. 2, No. 9, Dec/Jan 2004–2005):
  https://queue.acm.org/detail.cfm?id=1039523

- **Wikipedia: Object-Oriented Programming** (history section):
  https://en.wikipedia.org/wiki/Object-oriented_programming

- **Hillel Wayne — "Alan Kay Did Not Invent Objects"** (2019, critical nuance):
  https://www.hillelwayne.com/post/alan-kay/

- **Eric Elliott — "The Forgotten History of OOP"** (2019, Medium):
  https://medium.com/javascript-scene/the-forgotten-history-of-oop-88d71b9b2d9f

- **Object-composition Google Group thread: "Four pillars of OOP"** (discussion tracing the origin of the four pillars):
  https://groups.google.com/g/object-composition/c/SxzxiSTl12s
