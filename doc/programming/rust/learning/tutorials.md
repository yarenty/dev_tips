---
title: Rust tutorials (curated)
main_link: https://google.github.io/comprehensive-rust/
keywords: [rust, tutorials, comprehensive-rust, macros, learning]
status: reviewed
---

# Rust tutorials (curated)

**Main link:** <https://google.github.io/comprehensive-rust/>

## Summary

A small curated list of long-form Rust tutorials worth the time, headlined by Google's [Comprehensive Rust](https://google.github.io/comprehensive-rust/) — a 4-day course originally taught to Android team engineers, now an open-source mdBook with exercises, slides, and translations. Includes pointers to a deep-dive on Rust's macro system (Jonathan Strejc's "Procedural Macros" talk) and a quirky pen-test-flavoured walkthrough ("learning Rust for fun and backdoo-rs") that reads as a long-form unsafe-Rust + Windows-API exercise.

## Insight

[Comprehensive Rust](https://google.github.io/comprehensive-rust/) is the densest free curriculum out there: each day is a half-day to a full day of slides + exercises, and there's a full *Bare-Metal Rust* track plus an *Android* track. It's the resource to hand a programmer who has 4 days, mentor support, and prior systems experience. The macro talk is a niche but valuable companion once you start writing your own `derive(Macro)` rather than just calling them. The "learning Rust for fun" article is closer to security-research entertainment than tutorial, but it shows how Rust feels when you bend it for low-level Windows interop. For pure-syntax speed see [[simple_short_intro_to_rust]]; for project-driven practice see [[from_easy_to_advanced]]; for the canonical book see [[_to_learn]].

## Similar / related topics

- [The Rust Programming Language](https://doc.rust-lang.org/book/) — the canonical book; longer than this article-length list.
- [Rustlings](https://github.com/rust-lang/rustlings) — small fix-the-compiler-error exercises; pairs with the book.
- [Tour of Rust](https://tourofrust.com/) — interactive multi-page tour.
- [Exercism Rust track](https://exercism.org/tracks/rust) — mentored exercise track.
- [The Little Book of Rust Macros](https://veykril.github.io/tlborm/) — definitive macro reference.

## Internal links
<!-- reviewed -->
- [[simple_short_intro_to_rust]]
- [[from_easy_to_advanced]]
- [[_to_learn]]
- [[scope]]
- [[cheats]]
- [[README]]

## Keywords

`#rust` `#tutorials` `#comprehensive-rust` `#macros` `#learning`

## References / raw notes

- **Comprehensive Rust** (Google) — <https://google.github.io/comprehensive-rust/welcome.html>
  - 4-day course; Android, Bare-Metal, and Concurrency deep-dives included; multi-language translations.
- **Procedural macros walkthrough** (YouTube) — <https://www.youtube.com/watch?v=MWRPYBoCEaY>
- **Learning Rust for fun and backdoo-rs** (HN/Humanative blog) — <https://security.humanativaspa.it/learning-rust-for-fun-and-backdoo-rs/>
  - Long-form pen-test/unsafe-Rust walkthrough; Windows-API focused, more entertainment than syllabus.
