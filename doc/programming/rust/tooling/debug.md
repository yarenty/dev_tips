---
title: Debug
main_link: https://crates.io/crates/debug_stub_derive
keywords: [debug, rust, stub, derive]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Debug

**Main link:** <https://crates.io/crates/debug_stub_derive>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[starship]] — starship _(score 26.4)_
- [[programming/rust/tooling/bottom|bottom]] — bottom _(score 26.4)_
- [[derivative]] — Derivative _(score 22.4)_
- [[uclicious]] — UCLicious _(score 19.9)_
- [[programming/rust/bottom|bottom]] — bottom _(score 18.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#debug` `#tooling` `#rust` `#programming` `#stub` `#derive` `#crates` `#default`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Debug

## default

```rust
#[derive(Debug)]
pub struct PubStruct {
    a: bool,
    b: usize,
}
```

## debug_stub_derive

https://crates.io/crates/debug_stub_derive


https://docs.rs/debug_stub_derive/0.3.0/debug_stub_derive/


```rust
#[macro_use]
extern crate debug_stub_derive;

// A struct from an external crate which does not implement the `fmt::Debug`
// trait.
pub struct ExternalCrateStruct;

// A struct in the current crate which wants to cleanly expose
// itself to the outside world with an implementation of `fmt::Debug`.
#[derive(DebugStub)]
pub struct PubStruct {
    a: bool,
    // Define a replacement debug serialization for the external struct.
    #[debug_stub="ReplacementValue"]
    b: ExternalCrateStruct
}

assert_eq!(format!("{:?}", PubStruct {
    a: true,
    b: ExternalCrateStruct

}), "PubStruct { a: true, b: ReplacementValue }");
```

## derivative

I think most powerful - 
see [derivative](derivative.md)
