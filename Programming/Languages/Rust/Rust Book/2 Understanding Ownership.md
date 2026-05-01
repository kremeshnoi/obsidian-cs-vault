---
title: "Understanding Ownership - The Rust Programming Language"
source: "https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html"
created: 2026-03-01
---

#programming #programming-languages #rust #rust-book

Ownership is Rust’s most unique feature and has deep implications for the rest of the language. It enables Rust to make memory safety guarantees without needing a garbage collector, so it’s important to understand how ownership works. In this chapter, we’ll talk about ownership as well as several related features: borrowing, slices, and how Rust lays data out in memory.