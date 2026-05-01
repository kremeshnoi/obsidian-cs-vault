#math #logic-and-foundations #lambda-calculus

# Currying

## Definition

Currying transforms a function that takes multiple arguments into a chain of functions, each taking a single argument. Instead of calling `f(a, b, c)`, you call `f(a)(b)(c)`.

```
f : (A, B, C) → D
```
becomes
```
f : A → B → C → D
```

Each intermediate call returns a new function that "remembers" the arguments already applied.

### Currying vs Partial Application

These are related but distinct concepts:

| | Currying | Partial Application |
|---|---|---|
| What it does | Transforms `f(a, b, c)` into `f(a)(b)(c)` | Fixes some arguments, returns a function for the rest |
| Result arity | Always unary (one argument per call) | Any arity (may accept multiple remaining arguments) |
| Automatic? | Structural transformation of the function | Applied manually by supplying specific values |

Currying _enables_ partial application — once a function is curried, you can partially apply it by simply not providing all arguments.

> [!note]
> The distinction matters: currying is a structural transformation (always produces unary functions), while partial application is an act of fixing specific argument values. In practice, many developers use the terms interchangeably, but they describe different operations.

## Why It Matters

Currying aligns functions with the mathematical model of lambda calculus, where every function takes exactly one argument. In practice, it enables point-free style, function composition pipelines, and reusable specialized functions — all without mutation or temporary variables.

## In Programming

Currying lets you build a family of specialized functions from a single general one.

```rust
// Uncurried
fn add(a: i64, b: i64) -> i64 {
    a + b
}

// Curried — returns a closure capturing the first argument
fn add_curried(a: i64) -> impl Fn(i64) -> i64 {
    move |b| a + b
}

fn main() {
    add(2, 3); // 5

    add_curried(2)(3); // 5

    // Partial application via currying
    let increment = add_curried(1);
    increment(10); // 11
    increment(20); // 21
}
```

### Multi-argument Currying

```rust
fn multiply_curried(a: i64) -> impl Fn(i64) -> Box<dyn Fn(i64) -> i64> {
    move |b| {
        let ab = a * b;
        Box::new(move |c| ab * c)
    }
}

fn main() {
    let result = multiply_curried(2)(3)(4); // 24

    let times_six = multiply_curried(2)(3);
    times_six(4);  // 24
    times_six(10); // 60
}
```

Rust does not support variadic generics, so a fully generic `curry` utility (like those found in dynamically typed languages) cannot be written as a single function. Each arity must be handled explicitly or via macros.

> [!info]
> In languages like Haskell and OCaml, all functions are curried by default — `f a b c` is always `((f a) b) c`. The explicit currying shown here in Rust is only necessary because most mainstream languages default to multi-argument functions.

### Practical Uses

```rust
// Building specialized functions from general ones
fn filter(predicate: impl Fn(&i64) -> bool + 'static) -> impl Fn(Vec<i64>) -> Vec<i64> {
    move |list| list.into_iter().filter(|x| predicate(x)).collect()
}

// Point-free composition
fn map(f: impl Fn(i64) -> i64 + 'static) -> impl Fn(Vec<i64>) -> Vec<i64> {
    move |list| list.into_iter().map(|x| f(x)).collect()
}

fn main() {
    let get_evens = filter(|x| x % 2 == 0);
    let get_positive = filter(|x| *x > 0);

    get_evens(vec![1, 2, 3, 4, 5, 6]);    // [2, 4, 6]
    get_positive(vec![-2, -1, 0, 1, 2]);   // [1, 2]

    let double_all = map(|x| x * 2);
    double_all(vec![1, 2, 3]); // [2, 4, 6]
}
```

### Greeting Example

```rust
fn greet(greeting: &'static str) -> impl Fn(&str) -> String {
    move |name| format!("{}, {}!", greeting, name)
}

fn main() {
    let say_hello = greet("Hello");
    let say_hi = greet("Hi");

    assert_eq!(say_hello("Alice"), "Hello, Alice!");
    assert_eq!(say_hi("Bob"), "Hi, Bob!");
}
```
