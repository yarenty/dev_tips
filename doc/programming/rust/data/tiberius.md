---
title: tiberius
main_link: https://crates.io/crates/tiberius
keywords: [tiberius, data, rust, programming, tds, goals, asynchronous, connection]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **tiberius**.

# tiberius

**Main link:** <https://crates.io/crates/tiberius>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#tiberius` `#data` `#rust` `#programming` `#tds` `#goals` `#asynchronous` `#connection`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# tiberius

https://crates.io/crates/tiberius


A native Microsoft SQL Server (TDS) client for Rust.

## Goals
- A perfect implementation of the TDS protocol.
- Asynchronous network IO.
- Independent of the network protocol.
- Support for latest versions of Linux, Windows and macOS.
## Non-goals
- Connection pooling (use bb8, mobc, deadpool or any of the other asynchronous connection pools)
- Query building
- Object-relational mapping
