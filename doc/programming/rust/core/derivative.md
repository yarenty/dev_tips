---
title: derivative — better derive macros
main_link: https://mcarton.github.io/rust-derivative/latest/index.html
keywords: [derivative, derive, macros, debug, default, hash, rust]
status: reviewed
---

# derivative — better derive macros

**Main link:** <https://mcarton.github.io/rust-derivative/latest/index.html>

## Summary

`derivative` is a procedural-macro crate that augments Rust's standard `#[derive(...)]` so you can derive `Debug`, `Default`, `Hash`, `PartialEq`, `Eq`, etc. **with per-field overrides** — ignore a field, format it through a custom function, give it a non-default default, change comparison semantics, and so on. Maintained by [@mcarton](https://github.com/mcarton), it's the workhorse you reach for the moment the standard derives become too rigid but you don't want to write the trait impl by hand.

```toml
derivative = "2.2.0"
```

## Insight

The killer use case is **`Debug` on a struct that contains a non-`Debug` field** — typically an `Arc<dyn Trait>`, a `Box<dyn Provider>`, an FFI handle, or anything from a third-party crate that didn't bother to implement `Debug`. With stock `#[derive(Debug)]` your only options are a hand-written impl or a wrapper newtype; with `derivative` you slap `#[derivative(Debug = "ignore")]` on the offending field and move on. Same trick for `Default = "true"` (generates a `new()` constructor) and for `Hash`/`PartialEq` (skip a cache field, hash a derived value).

Caveats: it's a `proc-macro` so it costs compile time; the syntax (`#[derivative(Trait(option = "value"))]`) is its own little DSL you'll forget between uses; and for the increasingly common case of "I just want a default value other than `Default::default()`" the much smaller [`smart-default`](https://crates.io/crates/smart-default) crate is often a cleaner pick. Also worth knowing: `educe` covers similar ground with a slightly different syntax and is more actively maintained.

## Similar / related topics

- [smart-default](https://crates.io/crates/smart-default) — focused alternative for `#[derive(Default)]` with custom values.
- [educe](https://crates.io/crates/educe) — broader derive helper, more actively maintained than derivative.
- [derive_more](https://crates.io/crates/derive_more) — derives for `From`, `Into`, `Add`, `Display`, etc.
- [[../learning/_must_have|_must_have]] — companion crates index.

## Internal links
<!-- reviewed -->
- [[README]]
- [[reflection]]

## Keywords

`#rust` `#derivative` `#derive` `#macros` `#debug` `#default` `#hash`

## References / raw notes

- Docs: <https://mcarton.github.io/rust-derivative/latest/index.html>
- Crate: <https://crates.io/crates/derivative>

### Debug — ignore a field that isn't `Debug`

```rust
use derivative::Derivative;

#[derive(Derivative)]
#[derivative(Debug)]
struct SQLTableSource {
    #[derivative(Debug = "ignore")]
    provider: Arc<SQLFederationProvider>,
    table_name: String,
    schema: SchemaRef,
}
```

A custom formatter is also available:

```rust
// #[derivative(Debug(format_with = "path::to::my_fmt_fn"))]
// fn fmt(&T, &mut std::fmt::Formatter) -> Result<(), std::fmt::Error>;
```

### Default — generate a `new()` constructor

```rust
use derivative::Derivative;

#[derive(Debug, Derivative)]
#[derivative(Default(new = "true"))]
struct Foo {
    foo: u8,
    bar: u8,
}

println!("{:?}", Foo::new()); // Foo { foo: 0, bar: 0 }
```

### Hash & Default

The crate supports per-field overrides for `Hash`, `PartialEq`, `Eq`, `PartialOrd`, `Ord`, `Clone`, and `Copy` in the same `#[derivative(...)]` style — see the docs for the full grid.
