---
title: mockall
main_link: https://crates.io/crates/mockall
keywords: [tests, tooling, rust, programming, crates, test, mock, turmoil]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# mockall

**Main link:** <https://crates.io/crates/mockall>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#tests` `#tooling` `#rust` `#programming` `#crates` `#test` `#mock` `#turmoil`

## TODO

- This file contains **3 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# mockall

https://crates.io/crates/mockall

Mock objects are a powerful technique for unit testing software. A mock object is an object with the same interface as a real object, but whose responses are all manually controlled by test code. They can be used to test the upper and middle layers of an application without instantiating the lower ones, or to inject edge and error cases that would be difficult or impossible to create when using the full stack.

Statically typed languages are inherently more difficult to mock than dynamically typed languages. Since Rust is a statically typed language, previous attempts at creating a mock object library have had mixed results. Mockall incorporates the best elements of previous designs, resulting in it having a rich feature set with a terse and ergonomic interface. Mockall is written in 100% safe and stable Rust.


# tux

https://crates.io/crates/tux


Miscellaneous test utilities for unit and integration tests in Rust.

[dev-dependencies]
tux = { version = "0.2" }
The goal of this project is to support a variety of test scenarios that may be tricky to test, such as HTTP requests, executing binaries, testing complex output, etc.

There is no particular scope with the code utilities provided, other than being generally useful in a unit or integration test scenario.


HAS!!! temp dirs / files  and HTTP requests


# turmoil

https://crates.io/crates/turmoil

written by tokio team
https://tokio.rs/blog/2023-01-03-announcing-turmoil


Turmoil is a framework for testing distributed systems. It provides deterministic execution by running multiple concurrent hosts within a single thread. It introduces "hardship" into the system via changes in the simulated network. The network can be controlled manually or with a seeded rng.
