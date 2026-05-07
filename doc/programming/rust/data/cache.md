---
title: Cache — Moka and quick_cache
main_link: https://crates.io/crates/moka
keywords: [cache, moka, quick_cache, lru, concurrent, in-memory]
status: reviewed
---

# Cache — Moka and quick_cache

**Main link:** <https://crates.io/crates/moka>

## Summary

This article covers the two leading **in-process** Rust cache crates: `moka` (the modern default, inspired by Java's Caffeine) and `quick_cache` (the lightweight low-overhead alternative). Both provide bounded concurrent hash-map-style caches with eviction policies; `moka` adds time-to-live, time-to-idle, weight-based bounds, and async support; `quick_cache` deliberately omits those for ~10× lower per-op overhead. For function-result memoisation specifically, the older [`cached`](https://crates.io/crates/cached) crate is the canonical choice.

## Insight

**Default to `moka`** for any cache where you want TTL, async/await support, eviction notifications, or weight-based capacity (e.g. byte-bounded caches over heterogeneous values). It scales well to many threads, supports both sync (`moka::sync::Cache`) and async (`moka::future::Cache`) flavours, and the API mirrors Caffeine which has 10+ years of tuning behind it. Reach for `quick_cache` when you have a hot read path where the cache itself shows up in a profile and you can live without TTL and event listeners — typical for tight inner loops in a query planner, compiler cache, or HTTP middleware. Reach for `cached` when you specifically want the `#[cached]` proc-macro to memoise a function with one line. Don't reach for `lru` (the crate) unless you specifically need single-threaded LRU with a tiny dep tree — moka's eviction is more sophisticated.

## Similar / related topics

- [[map_cache_storage]] — DashMap concurrent map (the backing structure for many caches).
- `cached` — function-memoisation with `#[cached]` macro.
- `lru` — minimal single-threaded LRU.
- `mini-moka` — moka's smaller sibling.
- Java Caffeine — direct API inspiration for moka.
- Memcached / Redis — out-of-process equivalents.

## Internal links
- [[map_cache_storage]] — DashMap (concurrent map)
- [[memory_storage]] — evmap (eventually-consistent multi-map)
- [[compilation_cache]] — sccache (build cache; very different scope)

## Keywords

`#cache` `#moka` `#quick_cache` `#lru` `#concurrent`

## References / raw notes

### Moka

- Crate: <https://crates.io/crates/moka>
- Repo: <https://github.com/moka-rs/moka>

> Moka is a fast, concurrent cache library for Rust, inspired by the Caffeine library for Java. Cache implementations on top of hash maps that support full concurrency of retrievals and high expected concurrency for updates. Also provides a non-thread-safe cache for single-thread applications. Bounded with a best-effort entry-replacement algorithm.

### quick_cache

- Crate: <https://crates.io/crates/quick_cache>
- Repo: <https://github.com/arthurprs/quick-cache>

> Lightweight and high-performance concurrent cache optimised for low cache overhead.
>
> - Small overhead vs. a concurrent hash table.
> - Scan-resistant, high hit-rate caching policy.
> - Scales well with thread count.
> - No background threads.
> - 100% safe code.
> - Small dependency tree.
>
> Optimised for use cases where access time and overhead can add up to a significant cost. Features like TTL, cost weighting, or event listeners are **not** implemented — use Moka if you need them.
