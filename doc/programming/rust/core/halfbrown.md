---
title: HalfBrown — adaptive HashMap
main_link: https://crates.io/crates/halfbrown
keywords: [halfbrown, hashmap, vec, hashbrown, simd-json, wayfair, rust]
status: reviewed
---

# HalfBrown — adaptive HashMap

**Main link:** <https://crates.io/crates/halfbrown>

## Summary

`halfbrown` is a `HashMap`-API-compatible map that uses **two backends transparently**: a `Vec<(K, V)>` while the map is small (default ~32 entries), and full-fat [`hashbrown`](hashbrown.md) once it grows past that threshold. For workloads dominated by tiny maps — JSON object decoding being the canonical example — the linear scan over a contiguous `Vec` beats hashbrown by a wide margin (no hashing, no probing, no cache miss), while bigger maps still get hashbrown's asymptotic behaviour. It's maintained by Wayfair / the [`simd-json`](https://github.com/simd-lite/simd-json) authors, where it materially moved their parser benchmarks.

## Insight

Reach for `halfbrown` when you have **lots of small maps** with bounded but unknown size — JSON parsers, config loaders, request-header bags, per-entity attribute bags in ECSes, anywhere you'd otherwise feel guilty about all those `HashMap::new()` allocations. The break-even point depends on key type and hasher — for `String` keys with the default hasher, the crossover is somewhere around 16-32 entries; for cheap-to-compare integer keys, lower.

Tradeoffs vs alternatives:

| Choice                 | Best when                                              |
|------------------------|--------------------------------------------------------|
| `std::HashMap`         | Default. You don't know enough yet to pick anything else. |
| [`hashbrown`](hashbrown.md) | Same as std, plus `no_std` or custom hasher.       |
| **`halfbrown`**        | Many maps, mostly tiny, occasionally larger.           |
| [`indexmap`](https://crates.io/crates/indexmap) | Need insertion-order iteration.       |
| [`smallvec`](https://crates.io/crates/smallvec) `Vec<(K,V)>` + linear scan | You'd manually inline this anyway, want `Copy` keys, ≤8 entries. |
| [`hashlink`](https://crates.io/crates/hashlink) | Need LRU semantics.                              |

Gotchas: the sentinel "we just switched backends" allocation is a hidden cost — if your workload thrashes around the threshold, you're paying for the worst of both worlds. Iteration order is not stable across the backend transition. The crate borrows much of hashbrown's docs verbatim, so don't be surprised when the docs read identically in places.

## Similar / related topics

- [[hashbrown]] — the SwissTable hash map; halfbrown's "big" backend.
- [[tinyvec]] — same "small inline, switch to heap when bigger" trick, but for `Vec`.
- [smallvec](https://crates.io/crates/smallvec) — the unsafe-allowing cousin of tinyvec; sometimes used as the small backend in similar designs.
- [indexmap](https://crates.io/crates/indexmap) — order-preserving alternative built on hashbrown.
- [simd-json](https://github.com/simd-lite/simd-json) — the JSON parser whose benchmarks drove halfbrown's design.

## Internal links
<!-- reviewed -->
- [[README]]
- [[hashbrown]]
- [[tinyvec]]

## Keywords

`#rust` `#halfbrown` `#hashmap` `#data-structures` `#performance` `#simd-json`

## References / raw notes

- Crate: <https://crates.io/crates/halfbrown>
- Docs: <https://docs.rs/halfbrown/>
- simd-json (the parent project): <https://github.com/simd-lite/simd-json>

`halfbrown` is a hash-map implementation that **dynamically switches from a vector-backed backend to a hashbrown-backed backend** as the number of keys grows. The heavy lifting is in hashbrown — the docs and API are copied from there.

The two backends optimise for different scenarios:

- **Vector backend (small)** — entries stored contiguously as `Vec<(K, V)>`; lookups are linear scans. No hashing, no probing, very cache-friendly for ≲32 entries.
- **Hashbrown backend (large)** — full SwissTable; standard hash-map asymptotics.

The transition happens automatically on insert when the map crosses the threshold. From the caller's perspective the API matches `std::collections::HashMap`.
