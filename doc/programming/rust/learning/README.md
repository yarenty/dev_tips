---
title: Rust learning
keywords: [learning, rust, error-handling, books, tutorials, project-ideas, cheatsheet]
status: reviewed
---

# Rust learning

Curated entry-points, reading lists, project ladders, and the small set of error-handling crates ([[anyhow]] / [[thiserror]] / [[eyre]]) that show up in nearly every Rust project. This folder is the "where do I start / what should I read next?" lobby of the Rust section — for crate-deep reference material see the sibling [[../core/README|core]], [[../concurrency/README|concurrency]], and other Rust subfolders.

## Where to start

| If you... | Read |
|-----------|------|
| ...have never opened a Rust file | [[simple_short_intro_to_rust]] (½ hour) → [The Rust Book](https://doc.rust-lang.org/book/) |
| ...want a syntax/semantics map handy | [[cheats]] (cheats.rs) |
| ...want a syllabus of intermediate topics | [[scope]] |
| ...want long-form courses & talks | [[tutorials]] |
| ...want a curated reading shelf | [[_to_learn]] |
| ...want a project to practise on | [[from_easy_to_advanced]] (15 projects, beginner→advanced) or [[7_more_ideas]] |
| ...want a one-paragraph project idea | [[_todo_ideas]] |
| ...want crate discovery | [[_must_have]] (curated index) → [[github_map]] (visual atlas) |
| ...need ergonomic error handling | [[anyhow]] (apps) + [[thiserror]] (libs); [[eyre]] for prettier reports |
| ...need `Cargo.toml` snippets | [[cargo_toml]] |
| ...want a small performance trick | [[_tricks]] |

## Articles

### Intro material

- [[simple_short_intro_to_rust]] — fasterthanli.me's *A half-hour to learn Rust*.
- [[tutorials]] — Comprehensive Rust (Google) + macros + security/unsafe.
- [[from_easy_to_advanced]] — 15-project ladder from ZTM.
- [[7_more_ideas]] — flatter list of seven free-form project ideas.
- [[_todo_ideas]] — single-project idea (port a Python blockchain to Rust).

### Cheat sheets / reference

- [[cheats]] — cheats.rs single-page Rust reference.
- [[scope]] — topics syllabus / "what to study next" roadmap.
- [[cargo_toml]] — `Cargo.toml` snippets, profiles, optional features.
- [[_tricks]] — `println!` 5× speedup trick (capture-by-value formatter).

### Error handling

- [[anyhow]] — application-side error type with context.
- [[thiserror]] — derive-macro for typed library errors.
- [[eyre]] — `anyhow` fork with custom report renderers (`color-eyre`).

> **Note**: there is also `[[../core/error|core/error]]` covering eyre from the deeper "core libraries" angle. Use whichever side feels closer to your current question.

### Curation / discovery

- [[_must_have]] — index pointing at awesome-rust + the split children below.
- [[_to_learn]] — books and book-shaped websites worth bookmarking.
- [[github_map]] — anvaka's visual map of the GitHub ecosystem.
- [[xc_in_rust]] — markdown-defined task runner (Joe Davidson's `xc`).

## See also

- [[../README|programming/rust]] — folder root.
- [[../core/README|core]] — borrow checker, error types, `pin-project`, generational arenas, etc.
- [[../concurrency/README|concurrency]] — tokio, crossbeam, actors, dataflow.
- [[../io/README|io]], [[../net/README|net]], [[../web/README|web]], [[../gui/README|gui]], [[../tui/README|tui]], [[../cli/README|cli]] — domain-specific Rust crates.
- [[../tooling/README|tooling]] — formatters, linters, build tools.

## Keywords

`#rust` `#learning` `#tutorials` `#books` `#cheats` `#error-handling` `#project-ideas`
