---
title: evmap — eventually-consistent concurrent multi-map
main_link: https://github.com/jonhoo/evmap
keywords: [evmap, lock-free, eventually-consistent, multi-map, left-right]
status: reviewed
review_date: 2026/05/03
---

# evmap — eventually-consistent concurrent multi-map

**Main link:** <https://github.com/jonhoo/evmap>

## Summary

`evmap` (Jon Gjengset's lock-free eventually-consistent concurrent multi-value map) is a specialised concurrent map for **read-heavy** workloads where readers can tolerate stale data between explicit `flush()` calls. It uses the [`left-right`](https://crates.io/crates/left-right) primitive: two copies of the map, one for reads and one for writes, with reader handles never taking locks on the critical path. The map is multi-value (each key maps to a `Vec<V>`), and each side carries optional meta-info that becomes visible to readers on flush.

## Insight

Reach for `evmap` when (a) you have many more readers than writers, (b) reads must be lock-free / never block, and (c) you can tolerate "writes are visible after the next `flush()`". Canonical use cases: routing tables, feature flags, service registries, view-only caches behind a single ingest task. Single-writer is the easy mode — one writer thread, N readers, never any contention; multi-writer requires wrapping the WriteHandle in a Mutex which throws away most of the win. Compared to [[map_cache_storage|DashMap]]: DashMap gives you immediate consistency at the cost of per-shard locks; evmap gives you lock-free reads at the cost of staleness. Compared to `RCU` (kernel pattern): same idea, simpler API. Gotchas: the multi-value-Vec semantics surprise people — `insert(k, v)` *appends* to the Vec, doesn't replace; the project is in maintenance mode (Jon publishes occasionally); for new code, also evaluate `papaya` or `flurry`.

## Similar / related topics

- [[map_cache_storage|DashMap]] — sharded, immediately consistent (different trade).
- `left-right` — the underlying lock-free primitive evmap is built on.
- `arc-swap` — pointer-swap for whole-collection-replace patterns.
- RCU (Linux kernel) — same idea at the OS level.
- `flurry` / `papaya` — modern concurrent-map alternatives.

## Internal links
- [[map_cache_storage]] — DashMap (sibling concurrent map).
- [[cache]] — moka / quick_cache (cache-shaped use cases).
- [[../concurrency/README|Rust concurrency]]

## Keywords

`#evmap` `#lock-free` `#eventually-consistent` `#left-right` `#multi-map`

## References / raw notes

- Repo: <https://github.com/jonhoo/evmap>
- Crate: <https://crates.io/crates/evmap>

A lock-free, eventually consistent, concurrent multi-value map.

The map allows reads and writes to execute entirely in parallel with no implicit synchronisation overhead. Reads never take locks on the critical path; writes don't either, assuming a single writer (multi-writer is possible behind a Mutex). Backed by the `left-right` primitive.

The trade-off: **eventual consistency**. Writes are not visible to readers until the writer calls `WriteHandle::flush`. Writers choose how stale they let reads get — flush after every write to emulate a regular concurrent HashMap, or batch flushes to reduce sync overhead.

Multi-value: every key maps to a `Vec<V>`. This adds a layer of indirection but enables advanced uses; you can't emulate it on top of normal map semantics.

Each side carries customisable meta — useful e.g. to record the time of the last refresh.
