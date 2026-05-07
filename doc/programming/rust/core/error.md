---
title: Rust error handling — eyre, anyhow, thiserror
main_link: https://crates.io/crates/eyre
keywords: [error-handling, eyre, color-eyre, anyhow, thiserror, miette, rust]
status: reviewed
---

# Rust error handling — eyre, anyhow, thiserror

**Main link:** <https://crates.io/crates/eyre>

## Summary

Rust's standard library leaves error handling deliberately spartan — `Result<T, E>`, the `?` operator, the `Error` trait — so practically every real project pulls in one or two crates to make it ergonomic. The de-facto split: **`thiserror`** for typed errors in libraries, **`anyhow`** for opaque error reports in applications, and **`eyre`** (with `color-eyre`) when you want anyhow's ergonomics plus a customisable, prettier reporter — colourised, sectioned, span-aware reports for CLIs and servers. This article focuses on `eyre` as the central crate, with cross-links to its siblings.

## Insight

Conceptually, three crates cover almost every Rust error-handling need:

| Crate        | Role                                  | When to use                                         |
|--------------|---------------------------------------|-----------------------------------------------------|
| `thiserror`  | derive macro for `enum MyError`       | **Library code** — callers can `match` on variants. |
| `anyhow`     | dynamic boxed error + `Context` trait | **Application code** — you just want `?` to bubble. |
| `eyre`       | fork of `anyhow` with custom handler  | App code where the **report rendering** matters.    |
| `color-eyre` | the canonical `eyre` reporter         | Pretty TTY reports + backtraces + `tracing` spans.  |
| `miette`     | diagnostics-style alternative         | LSP-like reports with source spans (compilers, parsers). |

`eyre`'s killer feature is the **pluggable `EyreHandler`** trait: install one once at `main()` (`color_eyre::install()?`) and every `eyre::Report` gets the prettier rendering for free. Pair it with `tracing-error` to capture the active `tracing` span chain into the report — that combination is the closest Rust comes to a Python-style traceback.

Gotchas:

1. **Don't use `anyhow`/`eyre` in libraries.** The whole point of `thiserror` is letting downstream callers handle specific variants; an opaque `anyhow::Error` taking that decision away from your users is a footgun.
2. **`color_eyre::install()` must run once, early.** Calling it twice panics; forgetting it gives you the unstyled default.
3. **Downcasting works but is awkward** — `report.downcast_ref::<ConcreteErr>()` instead of `match`.
4. **`Send + Sync + 'static`** is required of any error you wrap; FFI handles and `Rc<...>` won't fit.

The core/learning split: this article is the **deeper "what crate solves what problem" overview**; [[../learning/eyre|learning/eyre]] is the lighter "use it as part of the must-have bundle" view. Both point at the same upstream.

## Similar / related topics

- [[../learning/anyhow|anyhow]] — the upstream project; same API as eyre, no pluggable handler.
- [[../learning/thiserror|thiserror]] — derive-macro for typed library errors; the library-side counterpart.
- [[../learning/eyre|learning/eyre]] — sibling article focused on the curated learning bundle.
- [color-eyre](https://crates.io/crates/color-eyre) — the canonical `EyreHandler` implementation.
- [miette](https://crates.io/crates/miette) — alternative reporter with rustc-style source-span diagnostics.
- [tracing-error](https://crates.io/crates/tracing-error) — captures `tracing` spans into eyre/anyhow reports.

## Internal links
<!-- reviewed -->
- [[README]]
- [[../learning/anyhow|anyhow]]
- [[../learning/thiserror|thiserror]]
- [[../learning/eyre|learning/eyre]]

## Keywords

`#rust` `#error-handling` `#eyre` `#color-eyre` `#anyhow` `#thiserror` `#tracing`

## References / raw notes

- eyre crate: <https://crates.io/crates/eyre>
- color-eyre: <https://crates.io/crates/color-eyre>
- repo: <https://github.com/eyre-rs/eyre>
- anyhow: <https://crates.io/crates/anyhow>
- thiserror: <https://crates.io/crates/thiserror>
- miette: <https://crates.io/crates/miette>

`eyre::Report` is a trait-object-based error type for ergonomic error handling and reporting. The crate is a fork of `anyhow` adding a customisable error-report renderer — see `eyre::EyreHandler` for the customisation hook.

```rust
use eyre::{eyre, Result, WrapErr};

fn main() -> Result<()> {
    color_eyre::install()?;

    let opt: Option<()> = None;
    let _ = opt
        .ok_or_else(|| eyre!("missing required value"))
        .wrap_err("while initialising subsystem X")?;
    Ok(())
}
```
