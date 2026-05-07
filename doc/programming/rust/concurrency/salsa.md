---
title: "Salsa — incremental, on-demand computation framework"
main_link: https://github.com/salsa-rs/salsa
keywords: [salsa, rust, incremental, memoization, query-system, rust-analyzer, compiler]
status: reviewed
---

# Salsa — incremental, on-demand computation framework

**Main link:** <https://github.com/salsa-rs/salsa>

Book: <https://salsa-rs.github.io/salsa/> · Tutorial: <https://salsa-rs.github.io/salsa/tutorial/ir.html>

## Summary

[Salsa](https://github.com/salsa-rs/salsa) is a generic Rust framework for **on-demand, incrementalized computation** — you describe your program as a graph of pure functions over typed inputs, and Salsa memoises every result, tracks the dependencies between them, and only recomputes the *minimum* set of nodes when an input changes. It's the same query-system pattern Niko Matsakis built for the original Rust compiler refactor and that now powers [`rust-analyzer`](https://rust-analyzer.github.io/) — every "go to definition", every type-on-hover, every diagnostic in the IDE is a Salsa query firing against a memoised graph that's been incrementally updated since your last keystroke.

## Insight

Reach for Salsa when you have a derived value that depends on many inputs and **most inputs change rarely**. The classic shape is an IDE / build system / language server, but the pattern fits anywhere you'd otherwise hand-roll a "did this input change? if so re-run that step" graph: dataflow analysers, incremental code generators, dependency-aware test runners, GUI state derivations. The core mental model is two query kinds:

- **Inputs** — base facts you set imperatively (`set_source_text(file, "...")`). Mutable, the only place writes happen.
- **Tracked functions** — pure functions of inputs (and other tracked functions). Salsa memoises every call by its arguments and tracks which inputs / sub-queries it actually read.

When you change an input, dependent queries are *invalidated, not recomputed*. They re-run lazily when something asks for the value, and Salsa's "early cutoff" optimisation skips re-running downstream queries when an upstream re-run produced the same result. This is what makes IDE response times feel fast: most edits don't actually change any of the abstract syntax tree's hash values past the first 50 lines.

Things to know:

1. **Two generations exist in the wild.** Salsa "classic" (the macro-heavy `0.16`-era API) and the newer `salsa-2022` / current `0.x` branch (`#[salsa::tracked]`, `#[salsa::input]`, far cleaner ergonomics). New code should use the current branch; older blog posts and `rust-analyzer` internals reference the classic API.
2. **Killer use case = re-compute one of N derived values when a small input changes.** Anything where re-running the whole pipeline is wasteful but tracking dependencies by hand is fiddly. Compilers, build systems, IDE language services, dependency-tracked notebooks (Marimo's lineage), Bazel-shaped query engines.
3. **Wrong tool for stream / bulk batch processing.** Salsa is *demand-driven* — values are computed on `query()`, not on `set()`. If you need push-based dataflow over unbounded streams, look at [[timely]] / Differential Dataflow instead. If you just want a thread-safe `HashMap` of memoised results, [`cached`](https://crates.io/crates/cached) or [`moka`](https://github.com/moka-rs/moka) is much lighter.
4. **It's a framework, not a library.** You commit to expressing your computation as `#[salsa::query]` / `#[salsa::tracked]` functions on a `Database` trait. That's a real architectural decision; bolting it onto an existing imperative codebase is painful.

## Similar / related topics

- [`rust-analyzer`](https://rust-analyzer.github.io/) — the largest deployment of Salsa in the wild; useful as a real-world reference architecture.
- [Adapton](http://adapton.org/) — earlier incremental computation framework (ML / Rust); academic precursor with similar dependency-tracking ideas.
- [Bazel](https://bazel.build/) / [Buck2](https://buck2.build/) — coarse-grained build-system-as-query-engine; incremental at the file/target level, where Salsa is incremental at the function-call level.
- [`cargo`'s incremental compilation](https://doc.rust-lang.org/rustc/codegen-options/index.html#incremental) — same problem domain (don't recompile what didn't change), but a fingerprint-based file-system implementation, not a query engine.
- [`cached`](https://crates.io/crates/cached) / [`moka`](https://github.com/moka-rs/moka) — when you want plain in-memory memoisation without the framework commitment.
- [[timely]] — push-based streaming dataflow; the dual of Salsa's demand-driven model.

## Internal links

<!-- reviewed -->

- [[timely]] — streaming dataflow contrast (push vs. pull / on-change vs. on-demand).
- [[concurrency/README|Rust concurrency]] — landing page; Salsa lives in the "incremental memoisation" row of the workload table.

## Keywords

`#rust` `#incremental` `#memoization` `#query-system` `#salsa` `#rust-analyzer` `#compiler`

## References / raw notes

### Key idea (from the upstream README)

Define your program as a set of queries. Each query behaves like a function `K -> V` mapping a key to a value. Two kinds:

- **Inputs** — base inputs to the system; you mutate them directly.
- **Functions** — pure functions of inputs (and other queries); results are memoised so they're not recomputed unless an underlying input has actually changed.

When inputs change, Salsa figures out (fairly intelligently) which memoised values can be reused and which must be recomputed.

### Useful entry points

- Repo: <https://github.com/salsa-rs/salsa>
- Book / overview: <https://salsa-rs.github.io/salsa/overview.html>
- Tutorial (build a tiny incremental IR): <https://salsa-rs.github.io/salsa/tutorial/ir.html>
- Niko Matsakis — "Encapsulating state in salsa" (blog series): <https://smallcultfollowing.com/babysteps/blog/categories/salsa/>
- `rust-analyzer` architecture (the real-world consumer): <https://rust-analyzer.github.io/book/contributing/architecture.html>
