---
title: Rust core
keywords: [rust, core, data-structures, error-handling, macros, async, pin, borrow-checker]
status: reviewed
---

# Rust core

The "core libraries and language tooling" lobby for the Rust section. Crates and concepts here are language-adjacent — they make day-to-day Rust pleasanter (better derives, prettier errors), expose underlying machinery (pin projection, ownership visualisation), or sit at the very-low-level data-structure layer (`hashbrown`, `tinyvec`). For project-level guidance and reading lists see [[../learning/README|learning]]; for async / threading see [[../concurrency/README|concurrency]].

## Articles

### Data structures

- [[hashbrown]] — the SwissTable hash map; std's `HashMap` since 1.36 is built on it.
- [[halfbrown]] — adaptive hash map: `Vec` backend small, hashbrown backend big.
- [[tinyvec]] — 100% safe `ArrayVec`/`TinyVec`; smallvec's audit-friendly cousin.
- [[generational_index]] — generational arenas (slotmap, thunderdome) — the ECS / compiler / GUI handle pattern.

### Macros & meta-programming

- [[derivative]] — better `#[derive(...)]` with per-field overrides (skip `Debug`, custom `Default`, etc.).
- [[reflection]] — Rust's reflection story: dtolnay's `reflect` (compile-time) + `Any` runtime patterns.

### Async machinery

- [[pin_project]] — safe pin-projection for hand-rolled `Future`/`Stream` impls.

### Error handling

- [[error]] — the eyre + anyhow + thiserror landscape. Sibling lighter intro at [[../learning/eyre|learning/eyre]].

### Tooling

- [[borrow_check]] — RustOWL: editor visualisation of ownership and lifetimes.

## When to reach for what

| Need                                                | Read                                  |
|-----------------------------------------------------|---------------------------------------|
| "Why is the borrow checker yelling at me?"          | [[borrow_check]] (RustOWL)            |
| "I want a better `#[derive(Debug)]`"                | [[derivative]]                        |
| "Lots of small `Vec`s in profiler hot paths"        | [[tinyvec]]                           |
| "Lots of small `HashMap`s"                          | [[halfbrown]]                         |
| "Stable handles into a graph / arena / ECS"         | [[generational_index]]                |
| "Handwriting a `Future` / `Stream`"                 | [[pin_project]]                       |
| "Pretty error reports / `?` everywhere"             | [[error]] (eyre/anyhow/thiserror)     |
| "Custom `#[derive]` macro design"                   | [[reflection]] (dtolnay's `reflect`)  |
| "Faster hashmap / `no_std` map"                     | [[hashbrown]]                         |

## See also

- [[../learning/README|learning]] — entry-points, reading lists, project ladders, `_must_have` index.
- [[../concurrency/README|concurrency]] — tokio, crossbeam, actors, dataflow.
- [[../tooling/README|tooling]] — formatters, linters, build helpers.
- [[../README|programming/rust]] — folder root.

## Keywords

`#rust` `#core` `#data-structures` `#error-handling` `#macros` `#async` `#pin` `#borrow-checker`
