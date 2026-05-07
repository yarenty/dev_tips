---
title: thiserror
main_link: https://crates.io/crates/thiserror
keywords: [thiserror, rust, error-handling, dtolnay, derive]
status: reviewed
---

# thiserror

**Main link:** <https://crates.io/crates/thiserror>

## Summary

`thiserror` is David Tolnay's derive macro for the standard `std::error::Error` trait. You declare an enum with one variant per failure mode, annotate each with `#[error("...")]`, and the macro generates `Display`, `From` conversions for `#[from]` fields, and the `source()` chain. The result is a typed, exhaustive error API for **library** code without the boilerplate of hand-writing `impl Error`.

## Insight

The rule of thumb in the Rust ecosystem is: **`thiserror` for libraries, [[anyhow]] for binaries**. Library consumers want to `match` on specific variants and recover; application code mostly wants to bubble-and-print. `thiserror` is a compile-time-only dependency (proc-macro), so it doesn't bloat your downstream users — they only see the generated `impl Error`. Two common gotchas: (1) `#[from]` only works when there's a 1:1 source-to-variant mapping; if multiple variants want to absorb the same upstream type you'll need an explicit `impl From`. (2) The transparent `#[error(transparent)]` attribute is what you want when wrapping a single inner error and you don't want to add prefix text.

## Similar / related topics

- [[anyhow]] — the application-side counterpart; pair them.
- [[eyre]] — anyhow-fork with custom report renderers.
- `snafu` — alternative derive crate emphasising context selectors.
- `displaydoc` — sister crate from dtolnay, derives `Display` from doc-comments.
- `miette` — diagnostics-oriented crate for compiler-style error reports.

## Internal links
<!-- reviewed -->
- [[anyhow]]
- [[eyre]]
- [[../core/error|core/error]]
- [[README]]

## Keywords

`#thiserror` `#rust` `#error-handling` `#dtolnay` `#derive` `#crates`

## References / raw notes

- crates.io: <https://crates.io/crates/thiserror>
- repo: <https://github.com/dtolnay/thiserror>

```toml
[dependencies]
thiserror = "1"
```

```rust
use thiserror::Error;

#[derive(Debug, Error)]
pub enum DataStoreError {
    #[error("data store disconnected")]
    Disconnect(#[from] std::io::Error),
    #[error("the data for key `{0}` is not available")]
    Redaction(String),
    #[error("invalid header (expected {expected:?}, found {found:?})")]
    InvalidHeader { expected: String, found: String },
    #[error("unknown data store error")]
    Unknown,
}
```
