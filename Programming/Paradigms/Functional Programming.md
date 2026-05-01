#programming #paradigms

# Functional Programming

## Definition

Functional programming describes computation as the **evaluation of expressions and composition of functions**. The programmer declares *what* the result should be, not *how* to compute it step by step.

Rooted in lambda calculus (Alonzo Church, 1930s), FP treats functions as first-class values — they can be passed, returned, and composed like any other data.

## Core Characteristics

**Immutability by default.** Data is not modified — new values are derived from existing ones through transformations.

**Functions as the primary building block.** Logic is expressed as small, composable functions rather than sequences of statements.

**Declarative intent.** Code reads as a description of the result, not as instructions for the machine.

**Side effects are isolated.** I/O and mutation are pushed to the boundaries of the system, keeping the core logic pure and testable.

## Qualitative Attributes

- **Single-level abstraction** — each function operates at one level of meaning.
- **High density per line** — a single line often expresses a complete transformation.
- **Low mutable state** — data flows through expressions, not through reassignment.
- **Composable transformations** — small functions chain into complex pipelines.

## Code Example

```rust
fn read_urls(path: &str) -> Vec<String> {
    std::fs::read_to_string(path)
        .unwrap()
        .lines()
        .filter(|line| !line.is_empty())
        .filter(|url| url.starts_with("http://"))
        .map(String::from)
        .collect()
}
```

Each transformation is a pure step: read → split into lines → discard empty → keep only HTTP → collect. No intermediate variables, no mutation. The function does one thing — extracts valid URLs from a file.

Fetching content is a separate responsibility with side effects, and belongs outside this pipeline:
```rust
let responses: Vec<String> = read_urls("urls.txt")
    .iter()
    .map(|url| fetch(url))
    .collect();
```

## Contrast with Imperative Style

The same logic expressed imperatively:
```rust
let file_content = std::fs::read_to_string("urls.txt").unwrap();
let file_urls: Vec<&str> = file_content.split('\n').collect();
let mut urls = Vec::new();
for url in &file_urls {
    if !url.is_empty() && url.starts_with("http://") {
        urls.push(url.to_string());
    }
}
```

The imperative version manages state explicitly — declares a mutable vector, iterates, checks conditions, pushes. The functional version expresses the same as a data pipeline without intermediate state.

## Key Concepts

**Pure functions** — same input always produces same output, no side effects. This makes code predictable, testable, and safe to parallelize.

**Higher-order functions** — functions that take or return other functions. `map`, `filter`, `fold` are the workhorses.

**Composition** — building complex behavior by chaining simple functions. The output of one becomes the input of the next.

**Referential transparency** — any expression can be replaced with its value without changing program behavior.

> [!tip]
> Referential transparency is what makes functional code easy to test and reason about — if you can replace any expression with its result, you can verify correctness locally without knowing the rest of the system.

## Trade-offs

### Pros
- Pure functions are trivial to test — no setup, no mocking, no state.
- Safe concurrency — immutable data cannot cause data races.
- Highly composable — small functions snap together like LEGO.
- Easier to reason about — each function is self-contained.

### Cons
- Performance overhead in some cases — immutable data means more allocations.
- Steeper learning curve — requires thinking in transformations, not steps.
- Debugging pipelines can be harder than stepping through statements.
- Some problems (stateful protocols, UI) are naturally imperative and awkward to express functionally.

## Why It Matters

FP is not about avoiding state — it is about **controlling where and when state changes**. By making purity the default and side effects explicit, functional code scales in complexity without scaling in unpredictability.

> [!info]
> The functional and imperative paradigms are not mutually exclusive. Most modern languages (Rust, Kotlin, TypeScript) blend both — using functional pipelines for data transformation and imperative code for I/O and side effects.