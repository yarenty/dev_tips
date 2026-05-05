---
title: ADBC !
main_link: https://github.com/apache/arrow-adbc/tree/main
keywords: [adbc, rust, arrow, api, sql, retrieval, databases]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **ADBC !**.

# ADBC !

**Main link:** <https://github.com/apache/arrow-adbc/tree/main>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[gluesql]] — GlueSQL _(score 27.0)_
- [[db]] — diesel _(score 27.0)_
- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 27.0)_
- [[mobc]] — mobc _(score 21.5)_
- [[barrel]] — barrel _(score 21.5)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#adbc` `#data` `#rust` `#programming` `#arrow` `#api` `#flight` `#sql`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# ADBC !


https://github.com/apache/arrow-adbc/tree/main



https://github.com/apache/arrow-adbc/blob/main/rust/src/lib.rs

ADBC: Arrow Database Connectivity

ADBC is an API standard (version 1.0.0) for database access libraries ("drivers") in C, Go, and Java that uses Arrow for result sets and query parameters. Instead of writing code to convert to and from Arrow data for each individual database, applications can build against the ADBC APIs, and link against drivers that implement the standard. Additionally, a JDBC/ODBC-style driver manager is provided. This also implements the ADBC APIs, but dynamically loads drivers and dispatches calls to them.

Like JDBC/ODBC, the goal is to provide a generic API for multiple databases. ADBC, however, is focused on bulk columnar data retrieval and ingestion through an Arrow-based API rather than attempting to replace JDBC/ODBC in all use cases. Hence, ADBC is complementary to those existing standards.

Like Arrow Flight SQL, ADBC is an Arrow-based way to work with databases. However, Flight SQL is a protocol defining a wire format and network transport as opposed to an API specification. Flight SQL requires a database to specifically implement support for it, while ADBC is a client API specification for wrapping existing database protocols which could be Arrow-native or not. Together, ADBC and Flight SQL offer a fully Arrow-native solution for clients and database vendors.

For more about ADBC, see the introductory blog post.

Status
ADBC versions the API standard and the implementing libraries separately.

The API standard (version 1.0.0) is considered stable, but enhancements may be made.

Libraries are under development. For more details, see the documentation.
