#math #abstract-algebra

# Associativity

## Definition

Associativity is the property of a binary operation where the **grouping of operands does not affect the result**. For any three elements `a`, `b`, and `c` in a set **S** with operation `•`:

```
(a • b) • c = a • (b • c)
```

When this holds, parentheses become irrelevant — you can write `a • b • c` without ambiguity.

Associativity is the defining property that separates a [[Semigroup]] from a [[Magma]]. A magma only guarantees closure; adding associativity yields a semigroup. Adding an [[Identity]] element on top gives a [[Monoid]].

```
Magma → Semigroup → Monoid → Group
(closure) (+assoc)  (+identity) (+inverse)
```

### Associative vs Non-Associative Operations

| Operation | Associative? | Example |
|---|---|---|
| Addition `+` | Yes | `(1 + 2) + 3 = 1 + (2 + 3) = 6` |
| Multiplication `×` | Yes | `(2 × 3) × 4 = 2 × (3 × 4) = 24` |
| String concatenation | Yes | `("a" + "b") + "c" = "a" + ("b" + "c") = "abc"` |
| Subtraction `−` | No | `(5 − 3) − 1 = 1` but `5 − (3 − 1) = 3` |
| Division `÷` | No | `(12 ÷ 4) ÷ 2 = 1.5` but `12 ÷ (4 ÷ 2) = 6` |
| Exponentiation `^` | No | `(2^3)^2 = 64` but `2^(3^2) = 512` |

Note that associativity and [[Commutativity]] are independent properties — an operation can be one without the other. Matrix multiplication is associative but not commutative. Subtraction is neither.

## Why It Matters

Associativity enables **parallel computation**. When grouping doesn't matter, a sequence of operations can be split across multiple workers and recombined in any order:

```
fold([a, b, c, d])
  = (a • b) • (c • d)       // split across two cores
  = a • (b • (c • d))       // right fold
  = ((a • b) • c) • d       // left fold
```

All produce the same result. Without associativity, the computation must proceed sequentially from left to right (or right to left), preventing parallelization.

This is why associativity is central to MapReduce, parallel reductions, streaming aggregations, and any distributed fold.

> [!tip]
> Before parallelizing a fold or reduction, always verify that your combining operation is associative. If it is not, splitting the work across threads will produce incorrect results.

## In Programming

Associativity determines whether you can safely parallelize a fold or change evaluation order.

```rust
fn main() {
    let nums = vec![1, 2, 3, 4, 5];

    // Addition is associative — left fold and right fold give the same result
    let left_fold = nums.iter().fold(0, |acc, x| acc + x);    // ((((0+1)+2)+3)+4)+5 = 15
    let right_fold = nums.iter().rfold(0, |acc, x| acc + x);  // 1+(2+(3+(4+(5+0)))) = 15
    assert_eq!(left_fold, right_fold);

    // Subtraction is NOT associative — order matters
    let left_fold = nums.iter().fold(0, |acc, x| acc - x);    // ((((0-1)-2)-3)-4)-5 = -15
    let right_fold = nums.iter().rfold(0, |acc, x| x - acc);  // 1-(2-(3-(4-(5-0)))) = 3
    assert_ne!(left_fold, right_fold);
}
```

### Parallel Reduction

```rust
fn parallel_sum(data: &[i64]) -> i64 {
    if data.len() <= 2 {
        return data.iter().sum();
    }
    let mid = data.len() / 2;
    let (left, right) = data.split_at(mid);

    // Safe to split because addition is associative
    let left_sum = parallel_sum(left);
    let right_sum = parallel_sum(right);

    left_sum + right_sum  // combine in any order
}

fn main() {
    let data = vec![1, 2, 3, 4, 5, 6, 7, 8];
    assert_eq!(parallel_sum(&data), 36);
}
```
