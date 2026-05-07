---
title: debug — `Debug` derive helpers (`debug_stub_derive`, `derivative`)
main_link: https://crates.io/crates/debug_stub_derive
keywords: [debug, rust, derive, derivative, debug-impl, foreign-types, ignore-fields]
status: reviewed
---

# debug — `Debug` derive helpers (`debug_stub_derive`, `derivative`)

**Main link:** <https://crates.io/crates/debug_stub_derive>

## Summary

This article is a small grab-bag covering the **two common pain points with `#[derive(Debug)]`**:

1. **A field's type comes from a foreign crate that doesn't `impl Debug`** — you can't `#[derive(Debug)]` your wrapper. → Use [`debug_stub_derive`](https://crates.io/crates/debug_stub_derive)'s `#[debug_stub="..."]` field attribute.
2. **You want to omit a noisy field, format it specially, or use a different `Debug` than the auto-derived one** — `#[derive(Debug)]` is all-or-nothing per type. → Use [`derivative`](https://crates.io/crates/derivative)'s much more powerful `#[derivative(Debug)]` with per-field options.

Of the two, **`derivative` is the one most projects reach for** — it covers more cases (Eq, Hash, Default too) and is more actively maintained. `debug_stub_derive` is the lighter single-purpose option. For broader debugging tooling (gdb/lldb, flamegraphs, tokio-console), see the cross-section pointers below — this article is specifically about the *`Debug` impl derive ergonomics* niche.

## Insight

### Default derive

```rust
#[derive(Debug)]
pub struct PubStruct {
    a: bool,
    b: usize,
}
```

That's all you usually need. The two cases below are when you can't.

### `debug_stub_derive` — for foreign types without `Debug`

```rust
#[macro_use]
extern crate debug_stub_derive;

// A struct from an external crate which does not implement fmt::Debug.
pub struct ExternalCrateStruct;

// We want our wrapper to expose a clean Debug impl.
#[derive(DebugStub)]
pub struct PubStruct {
    a: bool,
    #[debug_stub = "ReplacementValue"]
    b: ExternalCrateStruct,
}

assert_eq!(
    format!("{:?}", PubStruct { a: true, b: ExternalCrateStruct }),
    "PubStruct { a: true, b: ReplacementValue }"
);
```

### `derivative` — the more powerful option

```rust
#[derive(Derivative)]
#[derivative(Debug)]
pub struct Config {
    name: String,

    // hide a noisy / huge / sensitive field
    #[derivative(Debug = "ignore")]
    huge_blob: Vec<u8>,

    // format with a custom function
    #[derivative(Debug(format_with = "fmt_redacted"))]
    api_key: String,
}

fn fmt_redacted(_: &String, f: &mut std::fmt::Formatter) -> std::fmt::Result {
    f.write_str("***")
}
```

`derivative` also covers `Default` (`#[derivative(Default(value = "..."))]`), `Hash`/`PartialEq`/`Eq` (with `ignore` and bound-control), and `Clone` with custom bounds — useful for generics where the auto-derive's `T: Clone` bound is wrong.

**Modern alternative**: the standard library now has [`std::fmt::DebugStruct`](https://doc.rust-lang.org/std/fmt/struct.DebugStruct.html) for hand-rolled impls, and the `dbg!()` macro is good for one-off prints. For "redact secrets in Debug output", consider [`secrecy`](https://crates.io/crates/secrecy) (a `Secret<T>` newtype that has a hardcoded redacted Debug impl) — purpose-built for that exact use case.

### Other Rust debugging tools (out of scope here)

- `gdb` / `lldb` / `rust-gdb` / `rust-lldb` — wrap GDB/LLDB with Rust pretty-printers.
- [`cargo-flamegraph`](https://github.com/flamegraph-rs/flamegraph) — sampling profiler → SVG flamegraphs.
- `tokio-console` — async-runtime introspection (pairs with [[tracing]] via `console-subscriber`).
- [`bacon`](https://github.com/Canop/bacon) — file-watch + cargo diagnostics dashboard. See [[../../../tools/misc/bacon|bacon]].

## Similar / related topics

- [`derivative`](https://crates.io/crates/derivative) — the more powerful derive-helper.
- [`secrecy`](https://crates.io/crates/secrecy) — `Secret<T>` for redacting sensitive fields.
- [`displaydoc`](https://crates.io/crates/displaydoc) — derive `Display` from doc comments (sibling ergonomic crate).
- [[../core/derivative|core/derivative]] — the canonical Rust-core article on `derivative`.
- [`cargo-flamegraph`](https://github.com/flamegraph-rs/flamegraph), `tokio-console` — runtime debugging tools (different niche).

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[../core/derivative|core/derivative]] — canonical Rust-core article on derivative.
- [[tracing]] — pair `console-subscriber` for runtime task introspection.
- [[../../../tools/misc/bacon|bacon]] — watch-mode for cargo diagnostics.

## Keywords

`#debug` `#rust` `#derive` `#derivative` `#debug_stub_derive` `#secrecy` `#fmt`

## References / raw notes

- `debug_stub_derive`: <https://crates.io/crates/debug_stub_derive>, <https://docs.rs/debug_stub_derive/0.3.0/debug_stub_derive/>
- `derivative`: <https://crates.io/crates/derivative>
- See also `[[derivative]]` (canonical core article).
