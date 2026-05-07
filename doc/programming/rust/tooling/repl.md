---
title: repl — `evcxr` and `iRust`, REPLs for Rust
main_link: https://github.com/evcxr/evcxr
keywords: [repl, evcxr, irust, rust, jupyter, interactive, prototyping]
status: reviewed
---

# repl — `evcxr` and `iRust`, REPLs for Rust

**Main link:** <https://github.com/evcxr/evcxr>

## Summary

There is no built-in `cargo repl`, but two third-party Rust REPLs cover the niche:

- **[`evcxr`](https://github.com/evcxr/evcxr)** — David Lattimore's evaluation context for Rust. Run with `cargo install --locked evcxr_repl` and launch `evcxr`. Supports `:dep serde = "1"` to pull crates on the fly, multi-line entry, persistent variables across statements, and is the substrate behind [[rust_in_jupyter|`evcxr_jupyter`]].
- **[`IRust`](https://github.com/sigmaSd/IRust)** — Mahmoud Saleh's terminal Rust REPL with syntax highlighting, async support, hint completion, and racer-style autocomplete. Try without installing via Gitpod: <https://gitpod.io/#https://github.com/sigmaSd/IRust>.

## Insight

Rust's compile-and-run model means REPLs aren't the day-to-day workflow they are in Python — but they are surprisingly useful for:

- **Quick API exploration** — `:dep tokio = { version = "1", features = ["full"] }` then play.
- **Numerical experiments** in [[rust_in_jupyter|Jupyter via `evcxr_jupyter`]] — pair with `polars` and `plotters` for ad-hoc data work.
- **Teaching / interview prep** — show a chain of `let` statements building up a concept without reaching for a `main.rs` scaffold.
- **Debugging serialization** — `:dep serde_json` then poke at JSON shapes interactively.

**Pick `evcxr`** if you want the same engine that powers Jupyter; **pick `iRust`** if you want a more polished terminal UX (better syntax highlighting and completion). Both are slow to start (compile-on-eval) compared to a Python REPL — Rust isn't trying to be Python here.

**Gotchas**: every `:dep` triggers a real `cargo build` of that crate inside the REPL's hidden workspace; first use of a heavy crate (e.g. `tokio` with `full` features) takes 30+ seconds. Borrow-checker errors interrupt mid-sentence in a way that can feel jarring. State is per-process; killing the REPL loses everything.

## Similar / related topics

- [`IRust`](https://github.com/sigmaSd/IRust) — terminal REPL alternative.
- [[rust_in_jupyter]] — `evcxr_jupyter` kernel for Jupyter notebooks.
- [Rust Playground](https://play.rust-lang.org/) — browser-based, no install, single-snippet only.
- Python `ipython` / `bpython` — what Rust REPLs are not aspiring to be.

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[rust_in_jupyter]] — Jupyter kernel built on evcxr.
- [[../learning/_must_have|learning/_must_have]] — broader "what to install when learning Rust".

## Keywords

`#repl` `#evcxr` `#irust` `#rust` `#interactive` `#jupyter` `#prototyping`

## References / raw notes

- evcxr repo: <https://github.com/evcxr/evcxr>
- iRust repo: <https://github.com/sigmaSd/IRust>
- Crate (iRust): <https://crates.io/crates/irust>
- Try iRust without installing: <https://gitpod.io/#https://github.com/sigmaSd/IRust>
- Install: `cargo install evcxr_repl` or `cargo install irust`.
