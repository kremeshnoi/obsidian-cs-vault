#math #abstract-algebra

# Magma

## Definition

A magma is the simplest algebraic structure: a set **S** equipped with a single binary operation `•` that is **closed** — meaning the result always belongs to the same set.

```
• : S × S → S
```

Closure is the only requirement. The operation does not need to be associative, commutative, or have an identity element.

For any two elements `a` and `b` in `S`:

```
a • b ∈ S
```

This guarantees that combining any two values of the type always produces a valid value of the same type. No partial results, no escaping the set.

## Why It Matters

Magma sits at the base of the algebraic hierarchy:

```
Magma → Semigroup → Monoid → Group
         (+assoc)   (+identity) (+inverse)
```

Each step adds one law. A [[Semigroup]] is a magma where the operation is also [[Associativity|associative]]. A [[Monoid]] adds an [[Identity]] element on top of that.

In programming, recognizing that an operation forms at least a magma tells you that the type is closed under combination — you can safely chain operations without leaving the type.

> [!note]
> Closure is the property that type systems enforce automatically — if `op(T, T) -> T`, the compiler guarantees you stay in `T`. The value of naming this as "magma" is that it makes explicit what you do *not* get: associativity, identity, or commutativity.

### Common Magmas

| Magma | Set | Operation | Associative? |
|---|---|---|---|
| Natural numbers under subtraction | ℕ | `−` | No: `(5 − 3) − 1 ≠ 5 − (3 − 1)` |
| Integers under subtraction | ℤ | `−` | No |
| Floats under average | ℝ | `avg` | No: `avg(avg(2, 4), 6) ≠ avg(2, avg(4, 6))` |

Subtraction over integers is a classic magma — the operation is closed (subtracting two integers always yields an integer), but it is not associative.

## In Programming

A magma maps to a type with a binary operation that always returns the same type — closure is the only guarantee.

```rust
trait Magma {
    fn op(self, other: Self) -> Self;
}

impl Magma for i64 {
    fn op(self, other: Self) -> Self {
        self - other
    }
}

fn main() {
    let a: i64 = 10;
    let b: i64 = 3;
    let c: i64 = 1;

    // Closure holds: subtracting two i64 always produces i64
    let result = a.op(b); // 7

    // But associativity does NOT hold:
    let left  = (a.op(b)).op(c);  // (10 - 3) - 1 = 6
    let right = a.op(b.op(c));    // 10 - (3 - 1) = 8
    assert_ne!(left, right);
}
```
