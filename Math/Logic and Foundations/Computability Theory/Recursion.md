#math #logic-and-foundations #computability-theory

# Recursion

## Definition

Recursion is a technique where a function calls itself to solve a problem by breaking it into smaller subproblems of the same shape. Each recursive call works on a reduced input until it reaches a **base case** — a condition that returns a result directly without further recursion.

In functional programming, recursion replaces imperative loops (`for`, `while`) as the primary mechanism for iteration.

Every recursive function has two parts:

1. **Base case** — stops the recursion and returns a known value.
2. **Recursive case** — reduces the problem and calls itself.

```rust
fn factorial(n: u64) -> u64 {
    if n == 0 {
        return 1; // base case
    }
    n * factorial(n - 1) // recursive case
}
```

### Tail Recursion

A recursive call is in **tail position** when it is the very last operation in the function — nothing is done with the result after the call returns. Tail-recursive functions can be optimized by the compiler into loops, using constant stack space (Tail Call Optimization / TCO).

```rust
// Not tail-recursive: multiplication happens AFTER the recursive call
fn factorial(n: u64) -> u64 {
    if n == 0 { return 1; }
    n * factorial(n - 1)
}

// Tail-recursive: the recursive call IS the last operation
fn factorial_tail(n: u64, acc: u64) -> u64 {
    if n == 0 { return acc; }
    factorial_tail(n - 1, n * acc)
}
```

The accumulator pattern (`acc`) is the standard technique for converting recursion into tail recursion — it carries the intermediate result forward instead of building it up on the call stack.

> [!warning]
> Rust does not guarantee tail call optimization. Even tail-recursive functions may overflow the stack on deep inputs. For performance-critical code, consider converting to an explicit loop or using the `tramp` crate for trampolining.

### Common Recursive Patterns

**Linear recursion** — each call makes one recursive call on a smaller input.
```rust
fn sum(list: &[i64]) -> i64 {
    match list {
        [] => 0,
        [head, tail @ ..] => head + sum(tail),
    }
}
```

**Tree recursion** — each call branches into multiple recursive calls.
```rust
fn fib(n: u64) -> u64 {
    if n <= 1 { return n; }
    fib(n - 1) + fib(n - 2)
}
```

**Mutual recursion** — two or more functions call each other.
```rust
fn is_even(n: u64) -> bool {
    if n == 0 { return true; }
    is_odd(n - 1)
}

fn is_odd(n: u64) -> bool {
    if n == 0 { return false; }
    is_even(n - 1)
}
```

> [!note]
> Tree recursion (like naive Fibonacci) has exponential time complexity. Memoization or conversion to dynamic programming is almost always needed for practical use.

## Why It Matters

Recursion is the foundation of computation theory — it is one of the equivalent models of computability alongside Turing machines and lambda calculus. In practice, it is the natural way to express algorithms over recursive data structures (trees, graphs, nested collections) and self-similar problems (divide and conquer, backtracking). Understanding recursion is essential for reasoning about termination, correctness, and performance in both functional and imperative paradigms.

## In Programming

Recursion is the natural way to traverse recursive data structures like trees.

```rust
enum Tree<T> {
    Leaf(T),
    Node(Box<Tree<T>>, Box<Tree<T>>),
}

fn sum(tree: &Tree<i64>) -> i64 {
    match tree {
        Tree::Leaf(v) => *v,
        Tree::Node(left, right) => sum(left) + sum(right),
    }
}

fn depth<T>(tree: &Tree<T>) -> usize {
    match tree {
        Tree::Leaf(_) => 1,
        Tree::Node(left, right) => 1 + depth(left).max(depth(right)),
    }
}

fn main() {
    let tree = Tree::Node(
        Box::new(Tree::Leaf(1)),
        Box::new(Tree::Node(
            Box::new(Tree::Leaf(2)),
            Box::new(Tree::Leaf(3)),
        )),
    );

    assert_eq!(sum(&tree), 6);
    assert_eq!(depth(&tree), 3);
}
```

### Trade-offs

| Aspect | Recursion | Iteration |
|---|---|---|
| State | Passed via arguments | Mutated in variables |
| Termination | Base case | Loop condition |
| Stack usage | Grows with depth (unless TCO) | Constant |
| Expressiveness | Natural for trees, graphs, nested structures | Natural for flat sequences |
| FP alignment | Avoids mutation | Requires mutable state |

### When to Use

- Traversing recursive data structures (trees, graphs, nested JSON).
- Problems that decompose into self-similar subproblems (divide and conquer, backtracking).
- Functional codebases where mutation and loops are avoided.
- When tail recursion is available to prevent stack overflow.