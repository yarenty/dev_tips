---
title: Rust learning scope (topics roadmap)
main_link: https://doc.rust-lang.org/book/
keywords: [scope, rust, roadmap, learning, syllabus, traits, lifetimes, error-handling]
status: reviewed
review_date: 2026/05/03
---

# Rust learning scope (topics roadmap)

**Main link:** <https://doc.rust-lang.org/book/>

## Summary

A personal syllabus / roadmap of the intermediate-and-up Rust topics worth working through after the basics: polymorphism via generics & traits, advanced memory management (lifetimes, smart pointers, deref coercion), error-handling conventions, and the functional-style features (closures, iterators, combinators). It's a pointer to "what to study next", not a tutorial in itself; the canonical material lives in [The Rust Programming Language](https://doc.rust-lang.org/book/), [Rust by Example](https://doc.rust-lang.org/rust-by-example/), and the more advanced [Rustonomicon](https://doc.rust-lang.org/nomicon/).

## Insight

The list is roughly the demarcation point between "I can write a working CLI" and "I can read other people's library code without flinching". The four buckets are deliberately ordered: traits unlock most of the standard library, lifetimes unlock most of the borrow-checker conversations, error handling unlocks ergonomic application code (see [[anyhow]] / [[thiserror]] / [[eyre]]), and the functional/iterator chapter unlocks idiomatic Rust style. Use it as a self-quiz: if any sub-bullet feels fuzzy, that's the next thing to read about. For each bucket the canonical follow-up reads are *The Rust Programming Language* (chapters 10, 13, 15–16, 19) and *Rust for Rustaceans* by Jon Gjengset.

## Similar / related topics

- [The Rust Programming Language ("the book")](https://doc.rust-lang.org/book/) — the canonical learning path.
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/) — the same topics as runnable snippets.
- [Rust for Rustaceans](https://rust-for-rustaceans.com/) — Jon Gjengset's intermediate-to-advanced book.
- [Comprehensive Rust](https://google.github.io/comprehensive-rust/) — Google's 4-day course (see [[tutorials]]).
- [[from_easy_to_advanced]] — project-based variant of the same idea.

## Internal links
- [[tutorials]]
- [[simple_short_intro_to_rust]]
- [[from_easy_to_advanced]]
- [[anyhow]]
- [[thiserror]]
- [[_to_learn]]
- [[README]]

## Keywords

`#scope` `#rust` `#roadmap` `#learning` `#syllabus`

## References / raw notes

Topics worth getting comfortable with, in roughly this order:

- **Polymorphism with generics & traits**
  - generics, traits
  - trait bounds, trait objects (`dyn Trait`)
  - super-traits
  - static dispatch vs dynamic dispatch
  - derive macros
  - the trait families in the standard library (`Iterator`, `From`/`Into`, `Read`/`Write`, `Display`/`Debug`, …)
- **Advanced memory management**
  - concrete lifetimes
  - generic lifetime annotations
  - lifetimes in functions & structs
  - smart pointers (`Box`, `Rc`, `Arc`, `RefCell`, `Mutex`)
  - implicit deref coercion
- **Error handling**
  - `panic!` for unrecoverable errors
  - `Result` for recoverable ones; `?` for propagation
  - `Result` & `Option` combinators
  - multiple error types in one function ([[anyhow]] / [[thiserror]] / [[eyre]])
- **Functional features**
  - closures & function pointers
  - the iterator pattern
  - iterating over collections
  - combinators (`map`, `filter`, `fold`, `collect`, …)

…and more: macros (declarative + proc), `unsafe`, FFI, async/await, the type-state pattern.
