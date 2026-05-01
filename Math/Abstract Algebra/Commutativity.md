#math #abstract-algebra

# Commutativity

## Definition

Commutativity is the property of a binary operation where the **order of operands does not affect the result**. For any two elements `a` and `b` in a set **S** with operation `•`:

```
a • b = b • a
```

When this holds, you can swap the left and right sides freely without changing the outcome.

Commutativity and [[Associativity]] are independent properties — an operation can have one without the other:

| Operation | Associative? | Commutative? |
|---|---|---|
| Addition `+` | Yes | Yes |
| Multiplication `×` | Yes | Yes |
| String concatenation | Yes | No: `"ab" ≠ "ba"` |
| Matrix multiplication | Yes | No: `A × B ≠ B × A` in general |
| Subtraction `−` | No | No: `5 − 3 ≠ 3 − 5` |
| Average | No | Yes: `avg(2, 4) = avg(4, 2)` |

A commutative [[Monoid]] (associative + identity + commutative) is especially powerful — it enables both parallel computation (from associativity) and order-independent merging (from commutativity).

## Why It Matters

Commutativity guarantees **order independence** — the result is the same regardless of which operand comes first. This matters in:

- **Distributed systems** — messages and events may arrive out of order. Commutative operations (like set union, max, addition) produce consistent results regardless of arrival order. This is the foundation of CRDTs (Conflict-free Replicated Data Types).
- **Query optimization** — commutative joins and filters can be reordered by the query planner without changing results.
- **Parallel aggregation** — when combined with associativity, commutativity allows arbitrary partitioning and recombination of data.

Without commutativity, you must preserve the exact order of operations, which constrains parallelism and requires coordination in distributed settings.

> [!info]
> CRDTs (Conflict-free Replicated Data Types) rely on operations being both commutative and associative. This combination allows replicas to apply updates in any order and still converge to the same state — no coordination needed.

## In Programming

Commutativity determines whether operation order matters in your code.

```rust
use std::collections::HashSet;

fn main() {
    // Addition is commutative
    assert_eq!(3 + 5, 5 + 3);

    // String concatenation is NOT commutative
    let a = String::from("hello ");
    let b = String::from("world");
    assert_ne!(format!("{}{}", a, b), format!("{}{}", b, a));
    // "hello world" ≠ "worldhello "

    // Set union is commutative
    let a: HashSet<i32> = [1, 2, 3].into();
    let b: HashSet<i32> = [3, 4, 5].into();
    assert_eq!(&a | &b, &b | &a);  // {1, 2, 3, 4, 5}

    // Max is commutative
    assert_eq!(3_i32.max(7), 7_i32.max(3));  // 7
}
```

### Order-Independent Merge

```rust
use std::collections::HashMap;

// Merges two counters — commutative because addition is commutative
fn merge_counters(a: &HashMap<String, i64>, b: &HashMap<String, i64>) -> HashMap<String, i64> {
    let mut result = a.clone();
    for (key, value) in b {
        *result.entry(key.clone()).or_insert(0) += value;
    }
    result
}

fn main() {
    let counter_a: HashMap<String, i64> =
        [("x".into(), 1), ("y".into(), 2)].into();
    let counter_b: HashMap<String, i64> =
        [("y".into(), 3), ("z".into(), 4)].into();

    // Order doesn't matter — merge is commutative
    let ab = merge_counters(&counter_a, &counter_b);
    let ba = merge_counters(&counter_b, &counter_a);
    assert_eq!(ab, ba);  // {"x": 1, "y": 5, "z": 4}
}
```
