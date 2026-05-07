---
title: anyhow
main_link: https://crates.io/crates/anyhow
keywords: [anyhow, rust, error-handling, dtolnay]
status: reviewed
review_date: 2026/05/03
---

# anyhow

**Main link:** <https://crates.io/crates/anyhow>

## Summary

`anyhow` is David Tolnay's de-facto crate for ergonomic error handling in Rust **applications** (binaries, scripts, integration glue). It provides one trait-object-based error type, `anyhow::Error`, that can absorb any `std::error::Error` plus a `Context` extension trait for attaching human-readable context as errors propagate. Combined with the `?` operator and the `anyhow!` / `bail!` / `ensure!` macros it removes 90% of the boilerplate around error definitions in code that doesn't need a custom enum.

## Insight

Reach for `anyhow` in **applications** where you mostly want to bubble errors up to `main` (or an HTTP handler) and print a decent backtrace; reach for [[thiserror]] in **libraries** where downstream code needs to match on specific variants. The two are designed to be combined: define your library's typed errors with `thiserror`, then the binary that consumes them uses `anyhow::Result<T>` everywhere and `.context("while loading config")` on calls that need extra info. `color-eyre` (built on [[eyre]], a fork of anyhow) is the prettier alternative if you want themed reports. The main gotcha is that `anyhow::Error` erases the concrete type, so `match err { MyErr::Foo => … }` won't compile — you need `err.downcast_ref::<MyErr>()`.

## Similar / related topics

- [[thiserror]] — derive macro for library-side typed error enums; the canonical pairing.
- [[eyre]] / `color-eyre` — fork of anyhow with customisable reports (colours, spans, sections).
- `snafu` — older alternative emphasising context selectors and per-call-site errors.
- `failure` — predecessor crate, deprecated; you'll still see it in old codebases.
- Standard `Box<dyn Error + Send + Sync>` — what `anyhow::Error` essentially is, with ergonomics added.

## Internal links
- [[thiserror]]
- [[eyre]]
- [[../core/error|core/error]]
- [[README]]

## Keywords

`#anyhow` `#rust` `#error-handling` `#dtolnay` `#crates`

## References / raw notes

- crates.io: <https://crates.io/crates/anyhow>
- repo: <https://github.com/dtolnay/anyhow>
- Typical usage:

```rust
use anyhow::{Context, Result, bail, ensure};

fn load_config(path: &str) -> Result<Config> {
    let bytes = std::fs::read(path)
        .with_context(|| format!("reading config from {path}"))?;
    let cfg: Config = toml::from_slice(&bytes)
        .context("parsing config TOML")?;
    ensure!(cfg.workers > 0, "workers must be > 0");
    Ok(cfg)
}

fn main() -> Result<()> {
    let cfg = load_config("config.toml")?;
    if cfg.workers > 1024 { bail!("too many workers: {}", cfg.workers); }
    Ok(())
}
```
