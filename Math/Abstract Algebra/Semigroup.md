#math #abstract-algebra

# Semigroup

## Definition

Semigroup - it's a mathematical concept that can be applied in functional programming when working with custom data structures or objects.

A semigroup is a [[Magma]] with [[Associativity|associativity]]. In other words, a semigroup is a set equipped with a binary operation that combines two elements of the set to produce another element from the same set. This operation must be associative, meaning that for any three elements `a`, `b`, and `c` in the set, the following equation holds.

```
(a * b) * c = a * (b * c)
```

The expression `(a * b) * c = a * (b * c)` is not a semigroup itself, but a property that determines whether the operation `*` forms a semigroup. It is simply a check for associativity. It tells us whether the fundamental property of a semigroup holds.

## Why It Matters

A semigroup sits one step above a [[Magma]] in the algebraic hierarchy — it adds [[Associativity|associativity]]. Adding an [[Identity]] element to a semigroup yields a [[Monoid]], completing the structure needed for safe, parallelizable folding over collections.

```
Magma → Semigroup → Monoid → Group
(closure) (+assoc)  (+identity) (+inverse)
```

## In Programming

A semigroup is any type with an associative binary operation. Without an identity element, folding requires at least one value.

```rust
trait Semigroup {
    fn combine(self, other: Self) -> Self;
}

impl Semigroup for String {
    fn combine(self, other: Self) -> Self {
        self + &other
    }
}

fn combine_all<T: Semigroup>(first: T, rest: impl IntoIterator<Item = T>) -> T {
    rest.into_iter().fold(first, |acc, x| acc.combine(x))
}

fn main() {
    let result = combine_all(
        String::from("hello"),
        vec![String::from(" "), String::from("world")],
    );
    assert_eq!(result, "hello world");

    // Without an identity element, you need at least one value —
    // you cannot fold an empty collection.
    // Adding an identity makes the structure a Monoid.
}
```

> [!note]
> Rust's `Iterator::reduce` is the semigroup fold — it returns `Option<T>` because an empty iterator has no identity element to fall back on. By contrast, `Iterator::fold` requires an initial value, making it a monoid operation.
