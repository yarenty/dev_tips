---
title: GlueSQL
main_link: https://crates.io/crates/gluesql
keywords: [gluesql, rust, sql, databases]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **GlueSQL**.

# GlueSQL

**Main link:** <https://crates.io/crates/gluesql>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[db]] — diesel _(score 27.0)_
- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 27.0)_
- [[barrel]] — barrel _(score 21.5)_
- [[diesel]] — diesel _(score 18.5)_
- [[programming/rust/sql_engine/sqlparser|sqlparser]] — SQLparser _(score 17.5)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#gluesql` `#data` `#rust` `#programming` `#sql` `#database` `#crates` `#library`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# GlueSQL

https://crates.io/crates/gluesql


GlueSQL is a SQL database library written in Rust.
It provides a parser (sqlparser-rs), execution layer, and optional storages (sled or memory) packaged into a single library.
Developers can choose to use GlueSQL to build their own SQL database, or as an embedded SQL database using the default storage engine.
