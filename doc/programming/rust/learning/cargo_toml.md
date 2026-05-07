---
title: Cargo.toml tricks
main_link: https://doc.rust-lang.org/cargo/reference/manifest.html
keywords: [cargo-toml, rust, cargo, profiles, serde, manifest]
status: reviewed
---

# Cargo.toml tricks

**Main link:** <https://doc.rust-lang.org/cargo/reference/manifest.html>

## Summary

A grab-bag of practical `Cargo.toml` snippets that come up over and over: the standard set of "foundation" dependencies, release-profile tweaks that strip a binary by ~50%, dev-profile tweaks that keep debug builds usable, the canonical "must-have derives" for serializable structs, and the optional-feature pattern for making `serde` switchable. Also includes the compile-time `Send + Sync` trick.

## Insight

Most Rust projects share the same first 5–10 lines of dependencies (`tokio`, `serde`, `serde_json`, `chrono`, `log` + a logger, `anyhow`/`thiserror`); keeping a personal snippet of these saves typing on every new crate. The release-profile tweaks (`strip = true`, `panic = "abort"`, `lto = true`, `codegen-units = 1`, `opt-level = "s"|"z"`) are the standard binary-size-shrinking incantation — applying all of them together typically halves a release binary, at the cost of slightly longer build time and losing panic-unwinding (which most CLIs don't need anyway). The dev-profile dance (`opt-level = 1` for own code, `opt-level = 3` for `*` dependencies) is the trick that makes debug builds tolerable on big workspaces — Serde, regex, and image crates are several-times slower at the default `opt-level = 0`. Finally, the `is_normal::<T>()` pattern is a zero-cost compile-time assertion that a type implements `Send + Sync + Unpin`, useful as a regression test for public types.

## Similar / related topics

- [Cargo book — Profiles](https://doc.rust-lang.org/cargo/reference/profiles.html) — official reference for `[profile.*]` keys.
- [`cargo-bloat`](https://github.com/RazrFalcon/cargo-bloat) — finds what makes your binary big before you optimise.
- [`cargo-edit`](https://github.com/killercup/cargo-edit) — `cargo add/rm/upgrade` (folded into Cargo since 1.62, but still useful for `set-version`).
- [min-sized-rust](https://github.com/johnthagen/min-sized-rust) — exhaustive playbook for shrinking Rust binaries.
- [[cheats]] — broader cheat-sheet site (cheats.rs).

## Internal links
<!-- reviewed -->
- [[cheats]]
- [[_must_have]]
- [[README]]

## Keywords

`#cargo` `#cargo-toml` `#rust` `#profiles` `#serde` `#binary-size`

## References / raw notes

Standard starting block:

```toml
[dependencies]
chrono = "0.4"
log = "0.4.16"
simplelog = "0.12.0"
tokio = { version = "1", features = ["full"] }
serde = { version = "1.0", features = ["derive"] }
```

Shrink release binary by ~50%:

```toml
[profile.release]
strip = true        # remove symbols (skip if you're using cargo-bloat)
panic = "abort"     # drop panic-unwinding tables
codegen-units = 1   # compile crates serially so the optimiser sees more
lto = true          # link-time optimisation
opt-level = "s"     # optimise for size — try "z" for even smaller
```

Speed up debug builds (own code lightly optimised, deps fully):

```toml
[profile.dev]
opt-level = 1

[profile.dev.package."*"]
opt-level = 3
```

Optional `serde` (gate behind a feature so downstream users don't pay for it):

```toml
[dependencies]
serde = { version = "1.0", features = ["derive"], optional = true }
serde_json = "1.0"

[features]
serde = ["dep:serde"]
```

Standard derives + cfg-gated serde derive on the type:

```rust
#[cfg(feature = "serde")]
use serde::{Deserialize, Serialize};

#[derive(Debug, Clone, Default, PartialEq)]
#[cfg_attr(feature = "serde", derive(Deserialize, Serialize))]
pub struct UpdateUserRequest {
    pub email: Option<String>,
    pub first_name: Option<String>,
    pub last_name: Option<String>,
    #[cfg_attr(feature = "serde", serde(skip))]
    pub password: Option<String>,
}
```

Compile-time `Send + Sync + Unpin` assertion (zero-cost regression test):

```rust
fn is_normal<T: Sized + Send + Sync + Unpin>() {}

#[test]
fn normal_types() {
    is_normal::<UpdateUserRequest>();
}
```
