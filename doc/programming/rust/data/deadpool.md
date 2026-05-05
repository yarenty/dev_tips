---
title: deadpool
main_link: https://crates.io/crates/deadpool
keywords: [deadpool, data, rust, programming, pool, objects, managed, enabled]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **deadpool**.

# deadpool

**Main link:** <https://crates.io/crates/deadpool>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#deadpool` `#data` `#rust` `#programming` `#pool` `#objects` `#managed` `#enabled`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# deadpool

https://crates.io/crates/deadpool

Deadpool is a dead simple async pool for connections and objects of any type.

This crate provides two implementations:

Managed pool (deadpool::managed::Pool)

Creates and recycles objects as needed
Useful for database connection pools
Enabled via the managed feature in your Cargo.toml
Unmanaged pool (deadpool::unmanaged::Pool)

All objects either need to be created by the user and added to the pool manually. It is also possible to create a pool from an existing collection of objects.
Enabled via the unmanaged feature in your Cargo.toml
