#programming #programming-languages #rust

# Rust References (`&`) and Dereferencing (`*`)

## Core Concept

Every value in Rust lives at a specific **memory address**. References and dereferencing are two sides of the same coin:

- `&` — **take the address** of a value (borrow)
- `*` — **go to the address** and get the value (dereference / deref)

```rust
let value: i32 = 42;
let r: &i32 = &value; // r holds the address where 42 lives
let v: i32 = *r;      // follow the address, get 42
```

## Why References Exist

Rust uses **ownership** — each value has exactly one owner. Without references, passing a value to a function would **move** it:

```rust
fn print_length(s: String) {
    println!("{}", s.len());
}

let name = String::from("Something");
print_length(name);
// name is MOVED, can't use it anymore
```

References let you **borrow** without taking ownership:

```rust
fn print_length(s: &String) {
    println!("{}", s.len());
}

let name = String::from("Something");
print_length(&name);
// name is still valid here
```

## Immutable vs Mutable References

```rust
let mut x = 10;

let r = &x;       // immutable borrow — can read, can't modify
let r = &mut x;   // mutable borrow — can read AND modify

*r += 5;           // dereference and modify the original value
// x is now 15
```

## When You Need `*` (Explicit Deref)

### Comparing or computing with references

```rust
let a = 5;
let b = &a;

// let sum = a + b; won't compile: can't add i32 and &i32

let sum = a + *b; // deref first
```

### Inside iterators

`iter()` yields `&T`, not `T`. You often need to deref:

```rust
let nums = vec![1, 2, 3];

// x is &i32 here
nums.iter().fold(0, |acc, x| acc + *x);
```

## When You Don't Need `*` (Auto-Deref)

Rust automatically dereferences in many cases through the `Deref` trait.
### Method calls

```rust
let s = String::from("hello");
let r = &s;

r.len()   // auto-deref, same as (*r).len()
r.to_uppercase() // works without *
```
### `println!` and formatting

```rust
let x = 42;
let r = &x;

println!("{}", r);  // prints 42, not the address
println!("{}", *r); // same result
```

## Three Ways to "Remove" a Reference

These are equivalent and you'll see all three in real Rust code:

```rust
let nums = vec![1, 2, 3];

// 1. Deref at usage with *
nums.iter().map(|x| *x + 1).collect::<Vec<_>>();

// 2. Pattern matching in closure params with &
nums.iter().map(|&x| x + 1).collect::<Vec<_>>();

// 3. copied() — creates an iterator of values instead of references
nums.iter().copied().map(|x| x + 1).collect::<Vec<_>>();
```

## Pattern Matching with `&`

Destructuring a reference in a pattern is the **mirror** of dereferencing:

```rust
let reference: &i32 = &5;

// these two are the same:
let value = *reference;     // explicit deref
let &value = reference;     // pattern match: "this is a &something, give me the something"
```

This is commonly used in closures:

```rust
// without pattern matching
vec![1, 2, 3].iter().filter(|x| **x > 1);
//                          x is &&i32, need double deref

// with pattern matching
vec![1, 2, 3].iter().filter(|&&x| x > 1);
//                          unwrap both & layers
```

## Quick Reference

| Syntax          | Name                | What it does                             |
| --------------- | ------------------- | ---------------------------------------- |
| `&x`            | Reference / Borrow  | Takes the address of `x`                 |
| `&mut x`        | Mutable reference   | Takes a mutable address of `x`           |
| `*r`            | Dereference / Deref | Follows the address, gets the value      |
| `&x` in pattern | Destructure ref     | Unwraps a reference via pattern matching |

> [!summary] Rule of Thumb
> `&` **creates** a level of indirection. `*` **removes** a level of indirection. They are inverses of each other.