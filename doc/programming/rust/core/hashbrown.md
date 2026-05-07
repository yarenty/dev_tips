---
title: hashbrown — the fast HashMap behind std
main_link: https://github.com/rust-lang/hashbrown
keywords: [hashbrown, hashmap, swisstable, rust, std, amanieu]
status: reviewed
---

# hashbrown — the fast HashMap behind std

**Main link:** <https://github.com/rust-lang/hashbrown>

## Summary

`hashbrown` is a Rust port of Google's [SwissTable](https://abseil.io/about/design/swisstables) hash table, originally written by [@Amanieu](https://github.com/Amanieu). Since Rust 1.36 (2019) **`std::collections::HashMap` and `HashSet` are implemented on top of hashbrown** — so for most code you get its performance for free, and the standalone crate exists for two reasons: `no_std` users, and people who want the full hashbrown API surface (raw entry, `into_keys/values`, etc.) on older toolchains or with custom hashers.

## Insight

You normally **don't depend on `hashbrown` directly** — `std::collections::HashMap` is hashbrown under the hood, and the std hashmap defaults to the DoS-resistant SipHash. Reach for the crate explicitly when:

- **You need `no_std`** — `hashbrown` works without an allocator-backed std.
- **You want a faster hasher** — pair `hashbrown::HashMap` with [`ahash`](https://crates.io/crates/ahash), [`fxhash`](https://crates.io/crates/fxhash), or [`foldhash`](https://crates.io/crates/foldhash) for the classic "swap SipHash for something 2-5× faster, accept losing HashDoS resistance" trade. (Don't do this for any hash table whose keys come from untrusted user input.)
- **You want the raw entry API** for advanced patterns (e.g. avoiding double-hashing on insert-or-update).
- **You're building something downstream of `hashbrown`** like [`indexmap`](https://crates.io/crates/indexmap), [`dashmap`](https://crates.io/crates/dashmap), or [`halfbrown`](halfbrown.md) — they all build on hashbrown's table.

The SwissTable design itself is worth understanding if you care about cache behaviour: it uses an open-addressed table with **groups of 16 slots probed via SIMD** (a single `pcmpeqb`-class instruction matches the 7-bit hash tags for an entire group at once), giving very good cache locality and fast unsuccessful lookups. Matt Kulukundis's [CppCon 2017 talk](https://www.youtube.com/watch?v=ncHmEUmJZf4) is the canonical introduction.

Gotcha: the `hashbrown` crate's default hasher (`ahash`) is *not* the same as `std::collections::HashMap`'s default hasher (SipHash). Benchmarks comparing the two are usually really comparing hashers, not table designs.

## Similar / related topics

- [[halfbrown]] — adaptive map that starts as `Vec<(K,V)>` and switches to hashbrown above ~32 keys.
- [indexmap](https://crates.io/crates/indexmap) — order-preserving hash map; uses hashbrown internally.
- [dashmap](https://crates.io/crates/dashmap) — concurrent hash map; uses hashbrown shards internally.
- [ahash](https://crates.io/crates/ahash) / [foldhash](https://crates.io/crates/foldhash) — fast non-cryptographic hashers commonly paired with hashbrown.
- [SwissTable design talk](https://www.youtube.com/watch?v=ncHmEUmJZf4) — Matt Kulukundis, CppCon 2017.

## Internal links
<!-- reviewed -->
- [[README]]
- [[halfbrown]]
- [[tinyvec]]

## Keywords

`#rust` `#hashbrown` `#hashmap` `#swisstable` `#std` `#data-structures`

## References / raw notes

- Repo: <https://github.com/rust-lang/hashbrown>
- Crate: <https://crates.io/crates/hashbrown>
- Stdlib integration RFC: [rust-lang/rust#56241](https://github.com/rust-lang/rust/pull/56241)
- SwissTable: <https://abseil.io/about/design/swisstables>

`hashbrown` is now part of the official Rust `HashMap` — depending on the crate directly is mostly for `no_std`, custom-hasher, or raw-entry needs.
