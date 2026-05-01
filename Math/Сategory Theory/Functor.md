#math #category-theory

# Functor

## Definition

A functor is a structure that can be **mapped over**. It wraps a value in a context and provides a `map` operation that applies a function to the inner value without leaving the context.

Formally, a functor is a mapping between categories that preserves structure. In programming terms, it is a type `F<A>` with an operation:

```
map : (A → B) → F<A> → F<B>
```

`map` takes a function from `A` to `B` and lifts it to work inside the functor, transforming `F<A>` into `F<B>`.

### Functor Laws

Every valid functor must satisfy two laws:

**1. Identity** — mapping the identity function over a functor returns the same functor.
```
map(|x| x, fa) ≡ fa
```

**2. Composition** — mapping two functions in sequence is the same as mapping their composition.
```
map(g, map(f, fa)) ≡ map(|x| g(f(x)), fa)
```

These laws guarantee that `map` is predictable — it transforms the value inside without altering the structure.

> [!warning]
> A type that provides a `map` method but violates these laws is not a true functor. This matters because code that assumes law-abiding functors will behave unpredictably if the laws are broken.

## Why It Matters

Functors provide a uniform interface for transforming values inside any context — vectors, optional values, error results, and more. The `map` operation guarantees:

- The **shape** of the structure is preserved (a Vec stays a Vec, an Option stays an Option).
- The **transformation** is pure — no side effects, no surprises.
- **Composability** — you can chain transformations with confidence that the laws hold.

A functor is the prerequisite for more powerful abstractions: an **applicative functor** adds the ability to apply wrapped functions, and a [[Monad]] adds the ability to chain computations that themselves produce wrapped values.

## In Programming

### Vec and Iterator
```rust
let doubled: Vec<i32> = vec![1, 2, 3].into_iter().map(|x| x * 2).collect();
// [2, 4, 6]

let strings: Vec<String> = vec![1, 2, 3].into_iter().map(|x| x.to_string()).collect();
// ["1", "2", "3"]
```

### Custom functor: Wrapper
```rust
struct Wrapper<T>(T);

impl<T> Wrapper<T> {
    fn map<U>(self, f: impl FnOnce(T) -> U) -> Wrapper<U> {
        Wrapper(f(self.0))
    }

    fn inspect(&self) -> String
    where
        T: std::fmt::Debug,
    {
        format!("Wrapper({:?})", self.0)
    }
}

fn main() {
    let result = Wrapper(3)
        .map(|x| x + 1)
        .map(|x| x * 2);

    println!("{}", result.inspect()); // "Wrapper(8)"
}
```

### Option
```rust
// Option is Rust's built-in Maybe functor
let result = Some(5)
    .map(|x| x + 1)   // Some(6)
    .unwrap_or(0);     // 6

let result: Option<i32> = None;
let result = result
    .map(|x| x + 1)   // None — function never runs, no crash
    .unwrap_or(0);     // 0
```

> [!tip]
> `Option` as a functor is the key insight behind null safety — instead of checking for null before every operation, you `map` over the value and the `None` case propagates automatically.
