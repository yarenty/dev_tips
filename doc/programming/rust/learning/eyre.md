---
title: eyre
main_link: https://crates.io/crates/eyre
keywords: [eyre, color-eyre, rust, error-handling, dtolnay, jane-lusby]
status: reviewed
---

# eyre

**Main link:** <https://crates.io/crates/eyre>

## Summary

`eyre` is a fork of [[anyhow]] (originated by Jane Lusby) that exposes a pluggable `EyreHandler` so libraries and applications can install a **custom report renderer**. The most common pairing is `color-eyre`, which adds colourised, sectioned, backtraced, span-aware error reports — what you'd want from a Rust app's `main()` panic. The core API mirrors `anyhow` exactly: `eyre::Result<T>`, the `eyre!` / `bail!` / `ensure!` macros, and the `WrapErr` (née `Context`) extension trait.

## Insight

Reach for `eyre` (specifically `color-eyre`) when you want **prettier panics and error displays** than `anyhow`'s default — typical in CLIs, servers with structured logging, and anything that streams to a TTY. The split between this article and [[../core/error|core/error]] is roughly: this one is the "use it as part of the learning bundle" view, the core one is the deeper crate reference; both point at the same crate. Pair `color-eyre` with `tracing-error` to capture the active `tracing` span chain when an error bubbles up — that combination is the closest Rust comes to a Python-style traceback. The catch versus `anyhow`: an extra dependency, the report handler must be installed once at startup (`color_eyre::install()?`), and you can't downcast through the report handler the same way (use `eyre::Report::downcast_ref`).

## Similar / related topics

- [[anyhow]] — the upstream project; same API, no pluggable handler.
- `color-eyre` — the canonical handler implementation, with colours and sections.
- `tracing-error` — captures `tracing` spans into eyre/anyhow reports.
- [[thiserror]] — pair on the library side (typed) with eyre on the application side (report).
- `miette` — alternative diagnostics-style report crate (fancy LSP-like reports).

## Internal links
<!-- reviewed -->
- [[anyhow]]
- [[thiserror]]
- [[../core/error|core/error]]
- [[README]]

## Keywords

`#eyre` `#color-eyre` `#rust` `#error-handling` `#crates`

## References / raw notes

- eyre crate: <https://crates.io/crates/eyre>
- color-eyre: <https://crates.io/crates/color-eyre>
- repo: <https://github.com/eyre-rs/eyre>

```rust
use eyre::{eyre, Result, WrapErr};

fn main() -> Result<()> {
    color_eyre::install()?;

    let opt: Option<()> = None;
    let _ = opt.ok_or_else(|| eyre!("missing required value"))
        .wrap_err("while initialising subsystem X")?;
    Ok(())
}
```

> **Note:** the original raw-notes for this file (auto-split from `_must_have.md`) bundled together unrelated cookbook snippets for `dotenv`, `reqwest`, `lazy_static`, `chrono`, `tokio`, and the AWS SDK. They've been demoted out of this article — see the parent index [[_must_have]] for the curated landing list. Canonical references for the bundled topics:
>
> - dotenv: <https://crates.io/crates/dotenv> (now usually `dotenvy`, the maintained fork: <https://crates.io/crates/dotenvy>)
> - reqwest: <https://docs.rs/reqwest/latest/reqwest/>
> - lazy_static: <https://docs.rs/lazy_static/latest/lazy_static/> (largely superseded by `std::sync::LazyLock` in Rust 1.80+, or `once_cell::sync::Lazy`)
> - chrono: <https://crates.io/crates/chrono>
> - tokio: see [[../concurrency/tokio|concurrency/tokio]]
> - AWS SDK for Rust: <https://github.com/awslabs/aws-sdk-rust>
