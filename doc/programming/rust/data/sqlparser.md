---
title: sqlparser
main_link: https://crates.io/crates/sqlparser
keywords: [sqlparser, data, rust, programming, sql, parser, crates, specific]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **sqlparser**.

# sqlparser

**Main link:** <https://crates.io/crates/sqlparser>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#sqlparser` `#data` `#rust` `#programming` `#sql` `#parser` `#crate` `#specific`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# sqlparser

https://crates.io/crates/sqlparser

The goal of this project is to build a SQL lexer and parser capable of parsing SQL that conforms with the ANSI/ISO SQL standard while also making it easy to support custom dialects so that this crate can be used as a foundation for vendor-specific parsers.

This parser is currently being used by the DataFusion query engine, LocustDB, Ballista and GlueSQL.

This crate provides only a syntax parser, and tries to avoid applying any SQL semantics, and accepts queries that specific databases would reject, even when using that Database's specific Dialect. For example, CREATE TABLE(x int, x int) is accepted by this crate, even though most SQL engines will reject this statement due to the repeated column name x.
