#glossary #patterns

# Pattern

## Definition

A reusable, named solution template for a commonly occurring problem within a given context in software design.

## Key Properties

- **Name** — provides shared vocabulary for developers
- **Problem** — describes when to apply the pattern
- **Solution** — describes the structural approach, not a concrete implementation
- **Consequences** — trade-offs and implications of applying the pattern

## Context

Software Architecture, Object-Oriented Design, Functional Programming

## Categories

- **Creational** — object creation mechanisms ([[Singleton]], [[Factory]], [[Builder]])
- **Structural** — composition of classes and objects ([[Adapter]], [[Decorator]], [[Facade]], [[Proxy]], [[Composite]])
- **Behavioral** — communication between objects ([[Observer]], [[Strategy]], [[Command]], [[Iterator]], [[State]], [[Chain of Responsibility]], [[Mediator]])
- **Architectural** — system-level structure ([[CQRS]], [[Hexagonal Architecture]], [[MVC]], [[MVP]], [[MVVM]], [[Repository]], [[Event Sourcing]])
- **Concurrency** — coordinating parallel work ([[Producer-Consumer]])
- **Resilience** — fault tolerance and recovery ([[Circuit Breaker]], [[Retry]], [[Bulkhead]])

## Examples

[[Observer]], [[Strategy]], [[Decorator]], [[Monad]], [[Singleton]], [[Builder]], [[Command]]

## Related

[[Algorithm]], [[Architecture]], [[Anti-pattern]]

## Not to Be Confused With

[[Algorithm]] — an algorithm is a concrete, step-by-step procedure with deterministic output, while a pattern is a higher-level structural approach that may orchestrate algorithms but does not prescribe exact implementation.

> [!note]
> Patterns are not rules to apply universally — they are solutions that emerged from recurring problems. Applying a pattern without the matching problem leads to unnecessary complexity (see [[How Two Programmers Baked Bread]]).