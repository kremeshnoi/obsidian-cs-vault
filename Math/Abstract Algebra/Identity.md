#math #abstract-algebra

# Identity

## Definition

An identity element (also called a **neutral element**) is a special value `e` in a set **S** that, when combined with any other element using a binary operation `•`, leaves that element unchanged.

```
e • a = a       (left identity)
a • e = a       (right identity)
```

If both hold for the same element `e`, it is called a **two-sided identity**.

Given a [[Magma]] `(S, •)`, an element `e ∈ S` is an identity if and only if:

```
∀ a ∈ S:  e • a = a  ∧  a • e = a
```

When this element exists and the operation is also [[Associativity|associative]], the structure becomes a [[Monoid]].

### Common Identity Elements

| Operation | Set | Identity |
|---|---|---|
| Addition `+` | ℤ, ℝ | `0` |
| Multiplication `×` | ℤ, ℝ | `1` |
| String concatenation | Strings | `""` (empty string) |
| Vec concatenation | Vectors | `vec![]` (empty vec) |
| Boolean AND | `{true, false}` | `true` |
| Boolean OR | `{true, false}` | `false` |
| Function composition | Functions `a → a` | Identity function `|x| x` |

## Why It Matters

Identity is the key difference between a [[Semigroup]] and a [[Monoid]]. A semigroup only guarantees [[Associativity|associativity]], so folding an empty list is undefined. Adding an identity element makes the structure a monoid and gives you a safe, universally valid starting point for any accumulation.

## In Programming

The identity element provides a natural "starting value" for folds and reductions.

```rust
// Addition identity: 0
[1, 2, 3].iter().fold(0, |acc, x| acc + x);   // 6

// Multiplication identity: 1
[2, 3, 4].iter().fold(1, |acc, x| acc * x);   // 24

// String identity: ""
["a", "b", "c"].iter().fold(String::new(), |acc, x| acc + x);  // "abc"
```

Without an identity element you cannot safely fold an empty collection — there is no default value to return.

> [!tip]
> When implementing a custom type that supports accumulation, defining an identity value (via Rust's `Default` trait, for example) is what makes it safe to use with `fold`, `sum`, or `product` on empty inputs.
