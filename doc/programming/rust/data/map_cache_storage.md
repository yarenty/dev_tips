---
title: DashMap — concurrent hash map
main_link: https://crates.io/crates/dashmap
keywords: [dashmap, concurrent, hashmap, rwlock, sharded]
status: reviewed
---

# DashMap — concurrent hash map

**Main link:** <https://crates.io/crates/dashmap>

## Summary

`DashMap` is a fast concurrent hash-map for Rust — a near drop-in replacement for `RwLock<HashMap<K, V>>` that uses internal sharding so reads and writes can proceed in parallel without taking a single global lock. All mutating methods take `&self` (not `&mut self`), so a `DashMap` lives happily inside an `Arc<T>` shared across threads.

## Insight

Default to `DashMap` whenever you'd otherwise reach for `Arc<RwLock<HashMap<K, V>>>` and your access pattern is read-heavy or write-distributed. The sharding (default 4× CPU count) means contention only happens when two threads hit the same shard, which is rare for well-distributed keys. Gotchas: holding a `Ref`/`RefMut` keeps a shard locked — never `.await` while holding one, and never call back into the same `DashMap` from a closure that already holds a guard (deadlock!). For caches with eviction policies use `moka` instead — DashMap is unbounded by design. For lock-free *eventually-consistent* reads see `evmap`/[[memory_storage|evmap]]. For single-threaded hot paths a plain `HashMap` is still faster — the sharding overhead is a real cost.

## Similar / related topics

- `std::sync::RwLock<HashMap<...>>` — the simpler alternative; one global lock.
- [[cache|moka]] — DashMap-shaped but with TTL and bounded eviction.
- [[memory_storage|evmap]] — lock-free reads, eventually consistent (different trade).
- `flurry` — Rust port of Java's ConcurrentHashMap; another option.
- `papaya` — newer single-shard concurrent map; benchmarks well.

## Internal links
- [[cache]] — moka and quick_cache (cache-shaped DashMap usage).
- [[memory_storage]] — evmap (eventually-consistent alternative).
- [[../core/hashbrown|hashbrown]] — the SwissTable hash-map under DashMap and std.

## Keywords

`#dashmap` `#concurrent` `#hashmap` `#sharded`

## References / raw notes

- Crate: <https://crates.io/crates/dashmap>
- Repo: <https://github.com/xacrimon/dashmap>

> Blazingly fast concurrent map in Rust. DashMap is an implementation of a concurrent associative array/hashmap.
>
> DashMap tries to implement an easy-to-use API similar to `std::collections::HashMap` with slight changes for concurrency: all methods take `&self` so a DashMap can live in an `Arc<T>` shared across threads.

The original auto-detected `main_link` (`github.com/jonhoo/evmap`) was incorrect — that's the evmap repo, not DashMap. Corrected to crates.io/dashmap.
