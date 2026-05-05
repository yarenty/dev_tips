---
title: ROAPI
main_link: https://github.com/roapi/roapi/blob/main/columnq/src/query/graphql.rs
keywords: [roapi, rust, arrow, design, apache, sql]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# ROAPI

**Main link:** <https://github.com/roapi/roapi/blob/main/columnq/src/query/graphql.rs>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[datafusion]] — Datafusion SQL Query Planner _(score 26.6)_
- [[programming/rust/sql_engine/sqlparser|sqlparser]] — SQLparser _(score 21.5)_
- [[_to_learn]] — Books _(score 18.6)_
- [[db]] — diesel _(score 17.5)_
- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 17.5)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#roapi` `#sql-engine` `#rust` `#programming` `#datafusion` `#arrow` `#query` `#graphql`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# ROAPI

ROAPI automatically spins up read-only APIs for static datasets without requiring you to write a single line of code. It builds on top of Apache Arrow and Datafusion. The core of its design can be boiled down to the following:

Query frontends to translate SQL, GraphQL and REST API queries into Datafusion plans.
Datafusion for query plan execution.
Data layer to load datasets from a variety of sources and formats with automatic schema inference.
Response encoding layer to serialize intermediate Arrow record batch into various formats requested by client.


![](https://camo.githubusercontent.com/6846b7b40ddfc780fc97c2238fd7a4ea2594bde8883adf30fb0dc8086185c80d/68747470733a2f2f726f6170692e6769746875622e696f2f646f63732f696d616765732f726f6170692e706e67)

https://github.com/roapi/roapi/blob/main/columnq/src/query/graphql.rs
