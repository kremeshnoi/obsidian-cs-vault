#math #abstract-algebra

# Monoid

## Definition

A monoid is a [[Semigroup]] with an [[Identity]] element. In other words, it is a set **S** equipped with a binary operation `•` that satisfies three laws:

1. **Closure** — `a • b ∈ S` for all `a, b ∈ S`
2. **[[Associativity]]** — `(a • b) • c = a • (b • c)`
3. **Identity** — there exists `e ∈ S` such that `e • a = a • e = a`

```
(S, •, e)  where  • is associative and e is the identity
```

A monoid is the minimal algebraic structure that supports safe, parallelizable folding.

### Algebraic Hierarchy

```
Magma → Semigroup → Monoid → Group
(closure) (+assoc)  (+identity) (+inverse)
```

### Common Monoids

| Set | Operation | Identity |
|---|---|---|
| Integers | `+` | `0` |
| Integers | `×` | `1` |
| Strings | concatenation | `""` |
| Vectors | concatenation | `vec![]` |
| Booleans | `&&` | `true` |
| Booleans | `\|\|` | `false` |
| Functions `a → a` | composition | `|x| x` |

## Why It Matters

[[Associativity]] means the computation can be **split and parallelized** — the order of grouping does not affect the result:

```
fold([a, b, c, d])
  = fold([a, b]) • fold([c, d])     // split across two cores
  = (a • b) • (c • d)
```

The identity element means you can **safely fold an empty collection** — there is always a default value to return.

Together, these two properties make monoids the foundation for parallel reductions (MapReduce), streaming aggregations, incremental computation, and any scenario where data must be combined from independent sources.

> [!info]
> Whenever you see `fold(initial, combine)` in code, you are looking at a monoid in action — `initial` is the identity element and `combine` is the associative operation.

## In Programming

Monoids appear whenever you fold, reduce, or accumulate values.

```rust
trait Monoid: Clone {
    fn empty() -> Self;
    fn combine(&self, other: &Self) -> Self;
}

#[derive(Clone)]
struct Sum(i64);

#[derive(Clone)]
struct Product(i64);

impl Monoid for Sum {
    fn empty() -> Self { Sum(0) }
    fn combine(&self, other: &Self) -> Self { Sum(self.0 + other.0) }
}

impl Monoid for Product {
    fn empty() -> Self { Product(1) }
    fn combine(&self, other: &Self) -> Self { Product(self.0 * other.0) }
}

fn fold<T: Monoid>(values: &[T]) -> T {
    values.iter().fold(T::empty(), |acc, x| acc.combine(x))
}

fn main() {
    let nums = vec![Sum(1), Sum(2), Sum(3), Sum(4)];
    let total = fold(&nums);       // Sum(10)

    let nums = vec![Product(1), Product(2), Product(3), Product(4)];
    let total = fold(&nums);       // Product(24)

    let empty: Vec<Sum> = vec![];
    let total = fold(&empty);      // Sum(0) — safe on empty input
}
```
