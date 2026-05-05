---
title: Cache
main_link: https://crates.io/crates/moka
keywords: [cache, data, rust, programming, moka, crates, quick, concurrent]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/data/map_cache_storage.md` by `article_split.py`. Heading: **Cache**.

# Cache

**Main link:** <https://crates.io/crates/moka>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[map_cache_storage]] — DashMap _(score 51.2)_
- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 25.2)_
- [[barrel]] — barrel _(score 25.2)_
- [[gluesql]] — GlueSQL _(score 25.2)_
- [[programming/rust/sql_engine/sqlparser|sqlparser]] — SQLparser _(score 18.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#cache` `#data` `#rust` `#programming` `#moka` `#crates` `#quick` `#concurrent`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Cache

## Moka

https://crates.io/crates/moka


Moka is a fast, concurrent cache library for Rust. Moka is inspired by the Caffeine library for Java.

Moka provides cache implementations on top of hash maps. They support full concurrency of retrievals and a high expected concurrency for updates. Moka also provides a non-thread-safe cache implementation for single thread applications.

All caches perform a best-effort bounding of a hash map using an entry replacement algorithm to determine which entries to evict when the capacity is exceeded.





## quick cache

https://crates.io/crates/quick_cache


Lightweight and high performance concurrent cache optimized for low cache overhead.

- Small overhead compared to a concurrent hash table
- Scan resistent and high hit rate caching policy
- Scales well with the number of threads
- Doesn't use background threads
- 100% safe code in the crate
- Small dependency tree

- The implementation is optimized for use cases where the cache access times and overhead can add up to be a significant cost. Features like: time to live, cost weighting, or event listeners; are not current implemented in Quick Cache. If you need these features you may want to take a look at the Moka crate.
