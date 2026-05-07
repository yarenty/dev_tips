---
title: Rust books — curated reading list
main_link: https://doc.rust-lang.org/book/
keywords: [books, rust, reading-list, atomics, macros, performance, design-patterns, nomicon]
status: reviewed
---

# Rust books — curated reading list

**Main link:** <https://doc.rust-lang.org/book/>

## Summary

A small curated reading list of Rust books and book-shaped websites worth keeping bookmarked: the Rust Cookbook (recipes), Mara Bos's *Rust Atomics and Locks* (low-level concurrency), the Rust Design Patterns book, the Rustonomicon (the Dark Arts of `unsafe`), Lukas Wirth's *Little Book of Rust Macros*, Nick Nethercote's *Rust Performance Book*, and the GitHub mirror of Bos's atomics+locks book. They cover everything you'd need beyond [The Rust Programming Language](https://doc.rust-lang.org/book/) itself.

## Insight

These are the "level 2" reads — assume you've finished [The Rust Book](https://doc.rust-lang.org/book/) and want to go deeper in one direction. Rough mapping of when to reach for which: *Atomics and Locks* when you start writing lock-free code or care about memory ordering; the *Rustonomicon* when you start writing `unsafe`; *The Little Book of Rust Macros* the first time you write `macro_rules!`; the *Rust Performance Book* when `cargo bench` says you need it; the Cookbook when you remember "there must be a 5-line idiom for this"; the Design Patterns book for object-orientation patterns translated to Rust idioms (often: "use traits, not inheritance"). The DovAmir awesome-design-patterns repo is *language-agnostic* — it's a meta-list, not Rust-specific. See [[scope]] for what topics to study (this article is which books to read about them) and [[tutorials]] for the courses-and-talks angle.

## Similar / related topics

- [The Rust Programming Language ("the book")](https://doc.rust-lang.org/book/) — the canonical first book; assumed prerequisite.
- [Rust for Rustaceans](https://rust-for-rustaceans.com/) — Jon Gjengset's intermediate book; ownership/lifetimes/macros from a working-engineer angle.
- [Programming Rust (Blandy/Orendorff)](https://www.oreilly.com/library/view/programming-rust-2nd/9781492052586/) — O'Reilly's classic intermediate text.
- [Effective Rust](https://www.lurklurk.org/effective-rust/) — David Drysdale, modelled on Effective Java/C++.
- [Zero To Production In Rust](https://www.zero2prod.com/) — Luca Palmieri, project-based web-service walkthrough.

## Internal links
<!-- reviewed -->
- [[scope]]
- [[tutorials]]
- [[simple_short_intro_to_rust]]
- [[cheats]]
- [[README]]

## Keywords

`#books` `#rust` `#reading-list` `#atomics` `#macros` `#performance` `#nomicon` `#design-patterns`

## References / raw notes

- **Rust Cookbook** — <https://rust-lang-nursery.github.io/rust-cookbook/intro.html>
  Recipe-style how-do-I-do-X for the standard library + popular crates.
- **Rust Atomics and Locks** (Mara Bos) — <https://marabos.nl/atomics/>
  Low-Level Concurrency in Practice. The definitive book on memory ordering, atomics, and lock primitives in Rust.
  - GitHub mirror: <https://github.com/m-ou-se/rust-atomics-and-locks>
- **Rust Design Patterns** — <https://rust-unofficial.github.io/patterns/>
  Translates classic patterns to Rust idioms; includes anti-patterns and Rust-specific patterns (RAII, type-state, newtype, …).
  - Language-agnostic meta-list: <https://github.com/DovAmir/awesome-design-patterns>
- **The Rustonomicon** — <https://doc.rust-lang.org/nomicon/intro.html>
  The Dark Arts of Unsafe Rust: aliasing, drop semantics, raw pointers, FFI invariants.
- **The Little Book of Rust Macros** (Lukas Wirth) — <https://veykril.github.io/tlborm/introduction.html>
  Definitive reference for `macro_rules!` and (lightly) procedural macros.
- **The Rust Performance Book** (Nick Nethercote) — <https://nnethercote.github.io/perf-book/title-page.html>
  Practical playbook from the rustc / Firefox perf engineer.
