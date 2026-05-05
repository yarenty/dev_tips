---
title: DashMap
main_link: https://github.com/jonhoo/evmap
keywords: [map-cache-storage, data, rust, programming, moka, cache, crates, concurrent]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# DashMap

**Main link:** <https://github.com/jonhoo/evmap>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[cache]] — Cache _(score 48)_
- [[memory_storage]] — Memory storage _(score 28)_
- [[rtic]] — RTIC _(score 25)_
- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 23)_
- [[barrel]] — barrel _(score 23)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#map-cache-storage` `#data` `#rust` `#programming` `#moka` `#cache` `#crates` `#concurrent`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 2 additional top-level heading(s) extracted into sibling files:
> - [Memory storage](memory_storage.md)
> - [Cache](cache.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# DashMap

https://crates.io/crates/dashmap

Blazingly fast concurrent map in Rust.

DashMap is an implementation of a concurrent associative array/hashmap in Rust.

DashMap tries to implement an easy to use API similar to std::collections::HashMap with some slight changes to handle concurrency.

DashMap tries to be very simple to use and to be a direct replacement for RwLock<HashMap<K, V>>. To accomplish these goals, all methods take &self instead of modifying methods taking &mut self. This allows you to put a DashMap in an Arc<T> and share it between threads while still being able to modify it.

DashMap puts great effort into performance and aims to be as fast as possible. If you have any suggestions or tips do not hesitate to open an issue or a PR.
