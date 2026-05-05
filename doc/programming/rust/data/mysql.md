---
title: mysql
main_link: https://crates.io/crates/mysql
keywords: [mysql, data, rust, programming, pool, windows]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **mysql**.

# mysql

**Main link:** <https://crates.io/crates/mysql>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#mysql` `#data` `#rust` `#programming` `#support` `#pool` `#windows` `#see`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# mysql 
https://crates.io/crates/mysql

This crate offers:

- MySql database driver in pure rust;
- connection pool.

## Features:

- macOS, Windows and Linux support;
- TLS support via nativetls or rustls (see the SSL Support section);
- MySql text protocol support, i.e. support of simple text queries and text result sets;
- MySql binary protocol support, i.e. support of prepared statements and binary result sets;
- support of multi-result sets;
- support of named parameters for prepared statements (see the Named Parameters section);
- optional per-connection cache of prepared statements (see the Statement Cache section);
- buffer pool (see the Buffer Pool section);
- support of MySql packets larger than 2^24;
- support of Unix sockets and Windows named pipes;
- support of custom LOCAL INFILE handlers;
- support of MySql protocol compression;
- support of auth plugins:
  - mysql_native_password - for MySql prior to v8;
  - caching_sha2_password - for MySql v8 and higher.
