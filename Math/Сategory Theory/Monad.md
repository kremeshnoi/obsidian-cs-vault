#math #category-theory

# Monad

## Definition

A monad is a [[Functor]] that supports **chaining** — sequencing computations where each step depends on the result of the previous one, while staying inside a context.

A monad is a type `M<A>` with two operations:

```
of       : A → M<A>                     (also called unit, return, pure)
flat_map : (A → M<B>) → M<A> → M<B>    (also called bind, chain, >>=)
```

- `of` wraps a plain value into the monad.
- `flat_map` takes a function that returns a monad and chains it with an existing monad, flattening the result to avoid nesting (`M<M<B>>` → `M<B>`).

### Monad Laws

**1. Left identity** — wrapping a value and immediately chaining is the same as applying the function directly.
```
Some(a).and_then(f)  ≡  f(a)
```

**2. Right identity** — chaining with `Some` changes nothing.
```
m.and_then(Some)  ≡  m
```

**3. Associativity** — the order of nesting chains does not matter.
```
m.and_then(f).and_then(g)  ≡  m.and_then(|x| f(x).and_then(g))
```

### map vs and_then

The key difference from a [[Functor]]:

```rust
// map:      (A → B)    → M<A> → M<B>     — wraps the result automatically
// and_then: (A → M<B>) → M<A> → M<B>     — the function controls the wrapping
```

If you `map` with a function that returns a monad, you get nested contexts (`M<M<B>>`). `and_then` (called `flat_map` on iterators) flattens them into `M<B>`.

> [!tip]
> The rule of thumb: use `map` when your transformation returns a plain value, use `and_then`/`flat_map` when it returns a wrapped value. Choosing wrong gives you unwanted nesting.

## Why It Matters

Monads solve a fundamental problem: **how to compose functions that produce wrapped values**.

Without monads, chaining `A → M<B>` with `B → M<C>` would produce `M<M<C>>` — a nested mess. `and_then` / `flat_map` keeps everything at one level of wrapping, enabling clean, sequential composition.

This makes monads the standard abstraction for:
- **Error handling** — `Option`, `Result`: short-circuit on failure.
- **Collections** — `Iterator`: expand/filter elements through chained operations.
- **Asynchronous computation** — `Future`: sequence async steps.
- **Stateful computation** — State monad: thread state through pure functions.
- **I/O** — IO monad: describe side effects without executing them.

> [!info]
> Rust's `?` operator is syntactic sugar for monadic chaining on `Result` and `Option` — it calls `and_then` under the hood, short-circuiting on `Err`/`None` and unwrapping on `Ok`/`Some`.

## In Programming

### Option (safe chaining through None)
```rust
fn safe_divide(a: f64, b: f64) -> Option<f64> {
    if b == 0.0 { None } else { Some(a / b) }
}

fn main() {
    let result = Some(20.0)
        .and_then(|x| safe_divide(x, 4.0))
        .and_then(|x| safe_divide(x, 2.0))
        .unwrap_or(f64::NAN);
    // 2.5

    let result = Some(20.0)
        .and_then(|x| safe_divide(x, 0.0))   // None — short-circuits
        .and_then(|x| safe_divide(x, 2.0))   // never runs
        .unwrap_or(f64::NAN);
    // NaN
}
```

### Iterator (flat mapping over collections)
```rust
let result: Vec<i32> = vec![1, 2, 3]
    .into_iter()
    .flat_map(|x| vec![x, x * 10])
    .collect();
// [1, 10, 2, 20, 3, 30]

// vs map (would produce nested iterators):
let nested: Vec<Vec<i32>> = vec![1, 2, 3]
    .into_iter()
    .map(|x| vec![x, x * 10])
    .collect();
// [[1, 10], [2, 20], [3, 30]]
```

### Result (chaining fallible operations)
```rust
use std::num::ParseIntError;

fn parse(input: &str) -> Result<i64, ParseIntError> {
    input.trim().parse()
}

fn double_positive(n: i64) -> Result<i64, String> {
    if n <= 0 {
        Err("expected a positive number".into())
    } else {
        Ok(n * 2)
    }
}

fn main() {
    let result: Result<i64, String> = parse("21")
        .map_err(|e| e.to_string())
        .and_then(double_positive);
    // Ok(42)

    let result: Result<i64, String> = parse("-3")
        .map_err(|e| e.to_string())
        .and_then(double_positive);
    // Err("expected a positive number")
}
```
