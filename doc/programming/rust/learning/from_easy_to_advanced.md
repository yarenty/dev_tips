---
title: 15 Rust practice projects (beginner → advanced)
main_link: https://zerotomastery.io/blog/rust-practice-projects/
keywords: [project-ideas, rust, ladder, learning, ztm, beginner, intermediate, advanced]
status: reviewed
review_date: 2026/05/03
---

# 15 Rust practice projects (beginner → advanced)

**Main link:** <https://zerotomastery.io/blog/rust-practice-projects/>

## Summary

A 15-project ladder from Zero To Mastery, organised in three tiers — Beginner (CLI utilities, todo list, budget manager, unit converter), Intermediate (social-media bot, metered API, billing system, prescription system, spell checker, file-format parser, audio player, etc.), and Advanced (scripting language, ACID-compliant database, specialised compression algorithm). Beginner projects link out to similar reference implementations; intermediate projects deliberately don't, to push you to figure things out yourself; advanced ones come with no instructions at all.

## Insight

The pedagogy is deliberate: the linked references for beginner projects are crutches, the absence in intermediate ones is the point, and the absence in advanced ones is "if you can build this you know Rust". The "foundation crates" recommended at the top — `thiserror`, `color-eyre`, `clap`, `serde` — are exactly the ones you'd reach for in a real project, so the muscle memory transfers. Some specifics deserve modernisation: `warp` is mentioned for HTTP servers (still fine) but `axum` (the official tokio-rs successor) is now more common; the prescription-management section nicely promotes the **type-state pattern** as a way to encode workflow constraints in the type system, which is genuinely one of Rust's super-powers worth practising. Compare to [[7_more_ideas]] (a flatter, freer 7-item list) and [[_todo_ideas]] (a single-project port idea).

## Similar / related topics

- [[7_more_ideas]] — flatter list of 7 free-form project ideas.
- [[_todo_ideas]] — single-project Python-blockchain port.
- [Build your own X](https://github.com/codecrafters-io/build-your-own-x) — meta-list of clone-tutorials.
- [Codecrafters](https://codecrafters.io/) — paid platform with longer "build your own Redis/Git/SQLite" tracks.
- [Rustlings](https://github.com/rust-lang/rustlings) — fix-the-compiler-error exercises for fundamentals.

## Internal links
- [[7_more_ideas]]
- [[_todo_ideas]]
- [[scope]]
- [[tutorials]]
- [[README]]

## Keywords

`#project-ideas` `#rust` `#ladder` `#learning` `#ztm`

## References / raw notes

> Source: ZTM blog — *15 Rust Practice Projects* by Zachary Beuhler.
> URL: <https://zerotomastery.io/blog/rust-practice-projects/>

**Foundation crates** (used across most projects):

- [`thiserror`](https://crates.io/crates/thiserror) — custom error types ([[thiserror]])
- [`color-eyre`](https://crates.io/crates/color-eyre) — application error reports ([[eyre]])
- [`clap`](https://crates.io/crates/clap) — CLI argument parsing
- [`serde`](https://crates.io/crates/serde) — (de)serialisation

### Beginner projects (with reference impls)

1. **CLI utilities** — rewrite `echo`, `cat`, `ls`, `find`, `grep`. No extra crates needed beyond foundation. Stretch: extra flags, no `.unwrap()`, multi-threading on `find`/`grep`, colourised output. Reference: [uutils/coreutils](https://github.com/uutils/coreutils).
2. **Todo list** — add/remove/edit, mark done, save/load. Crates: `console`, `dialoguer`, `tui`, `rusqlite`. Stretch: full interactivity, `$EDITOR` integration, persist to a DB.
3. **Budget manager** — budgets + transactions + reporting. Crates: same as todo. Stretch: web API, near-budget warnings, time-bounded budgets, summary reports.
4. **Unit converter** — flag-driven CLI. Stretch: new types per unit, `From` impls, expression mode (`10C -> F`), ambiguous-unit error messages. Reference: [rink-rs](https://github.com/tiffany352/rink-rs).

### Intermediate projects (no walkthroughs)

5. **Social media bot** — Discord/Reddit/Twitter. Crates: `reqwest`, `serenity` (Discord), `kuon` / `twitter-api`, `reddit`. Stretch: full automation, command parser, external event source.
6. **Metered API server** — API-key issuance + usage tracking. Crates: `sqlx`, `warp` (or `axum` today). Stretch: usage reports, quotas, rate limiting.
7. **Custom data structures** — ordered HashMap, concurrent queue, BST, persistent. Crate: `petgraph` for graphs. Stretch: docs, benchmarks vs ecosystem.
8. **Utility billing system** — accounts + readings + bills + due dates. Crates: `sqlx`, `console`, `dialoguer`, `tui`, `axum`. Stretch: web UI, customer self-service, notifications.
9. **Prescription management system** — type-state pattern for workflow correctness; CRUD + refills + consult tracking. Crates: `sqlx`, `console`, `dialoguer`, `tui`, `axum`. Stretch: a long acceptance-test list (refills only with remaining count, partial-refill quantity caps, etc.).
10. **Spell checker crate** — input string → correct? Crate: `unicode-segmentation`. Stretch: bloom filter for memory, fuzzy-match suggestions, [Rust API guidelines](https://rust-lang.github.io/api-guidelines/) review, benchmarks.

### Advanced projects (no hand-holding)

11. **File-format parser** — pick a format (start with INI; level up to a binary format or programming language). Crate: `nom`. Stretch: real types in the AST, ecosystem benchmarks, test suite.
12. **Audio player** — file formats, encoding/decoding, DSP, GUI, real-time. GUI: `egui`. Discovery: <https://rust.audio/>. Stretch: playlists, seeking, library, parametric EQ, visualisations.
13. **Scripting language** — design and implement an interpreter. Crate: `nom`. Stretch: editor syntax highlighting, embed back into Rust.
14. **ACID-compliant database** — KV, time-series, or document. Start minimal. Stretch: query system, concurrent access.
15. **Specialised compression algorithm** — pick a data domain, design + implement. Crate: `itertools`; `slice::windows` / `slice::chunks` from std. Stretch: benchmarks, parallelisation.

**Bonus**: build a full-stack Twitter clone in Rust (ZTM members get the walkthrough).

(Ad copy from the original article omitted.)
