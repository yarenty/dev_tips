---
title: bb8
main_link: https://crates.io/crates/bb8
keywords: [bb8, data, rust, programming, connection, connections, database, crates]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **bb8**.

# bb8

**Main link:** <https://crates.io/crates/bb8>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#bb8` `#data` `#rust` `#programming` `#connection` `#connections` `#database` `#crates`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# bb8

https://crates.io/crates/bb8

full-featured connection pool, designed for asynchronous connections (using tokio). Originally based on r2d2.

Opening a new database connection every time one is needed is both inefficient and can lead to resource exhaustion under high traffic conditions. A connection pool maintains a set of open connections to a database, handing them out for repeated use.

bb8 is agnostic to the connection type it is managing. Implementors of the ManageConnection trait provide the database-specific logic to create and check the health of connections.

A (possibly not exhaustive) list of adapters for different backends:

| Backend |	Adapter Crate |
|-----|-----|
| tokio-postgres	| bb8-postgres (in-tree)
| redis	| bb8-redis (in-tree)
| rsmq	| rsmq_async
| bolt-client	| bb8-bolt
| diesel	| bb8-diesel
| tiberius	| bb8-tiberius
| nebula-client	| bb8-nebula
| memcache-async	| bb8-memcached
| lapin	| bb8-lapin
