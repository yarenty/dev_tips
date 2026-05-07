---
title: "Programming"
keywords: [programming, languages, runtime, tooling, build-systems, polyglot]
status: reviewed
review_date: 2026/05/03
---

# Programming

A grab-bag of language-, runtime-, and tooling-related notes. Honest framing up front: this section is **massively Rust-heavy**. The `rust/` subtree dominates the rest of `programming/` by something like a 50-to-1 ratio in article count, and most of the other-language sections here are placeholder landings with one or two articles each. That reflects what I actually reach for day-to-day, not a judgment about which languages are interesting.

## How to navigate

- **"I want to learn language X from scratch"** → follow the external pointers in the per-language README. None of these sections try to be a tutorial.
- **"I have a Rust question"** → the `rust/` subtree is the deep-dive. See [[programming/rust/README|Rust]].
- **"I want a tool/library that solves problem Y"** → cross-section links: databases live under `db/`, ML frameworks under `ml/`, dev/ops tooling under `tools/`.

## Subsections

- [[programming/caching/README|caching]] — in-process and external caches (Garnet, Redis-compatible servers).
- [[programming/cpp/README|C++]] — modern C++ notes: ABI, the Circle compiler.
- [[programming/go/README|Go]] — sparse landing; pointers to canonical Go resources.
- [[programming/java/README|Java]] — sparse landing; modern JDK / Kotlin / GraalVM pointers.
- [[programming/package_managers/README|package managers]] — sparse cross-language landing.
- [[programming/php/README|PHP]] — modern PHP tooling, primarily the Mago analyzer.
- [[programming/rust/README|Rust]] — by far the largest subtree.
- [[programming/scala/README|Scala]] — small landing, sbt notes.
- [[programming/swift/README|Swift]] — sparse landing.
- [[programming/unix_systems/README|unix systems]] — small curiosity collection of from-scratch OS projects.

## Articles

- [[assembly]] — curated entry-point reading list for learning x86/ARM assembly from zero.

## See also (cross-section)

- Databases: see the `db/` tree (Postgres, Redis, vector DBs, time-series).
- Tooling: see the `tools/` tree (terminals, editors, network utilities).
- ML / data: see the `ml/` and `viz/` trees.

## Keywords

`#programming` `#languages` `#runtime` `#tooling` `#polyglot` `#build-systems`
