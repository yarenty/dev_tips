---
title: SQLFlow
main_link: https://github.com/turbolytics/sql-flow?utm_source=tldrdata
keywords: [sqlflow, tools, db, duckdb, data, kafka, sql]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# SQLFlow

**Main link:** <https://github.com/turbolytics/sql-flow?utm_source=tldrdata>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[iceberg]] — Iceberg _(score 20)_
- [[postgresql]] — postgresql _(score 20)_
- [[quary]] — Quary _(score 18)_
- [[indexing]] — Indexing _(score 18)_
- [[cdc]] — CDC _(score 15)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#sqlflow` `#tools` `#db` `#duckdb` `#data` `#kafka` `#sql`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# SQLFlow


https://github.com/turbolytics/sql-flow?utm_source=tldrdata

## SQLFlow: DuckDB for Streaming Data.


SQLFlow is a high-performance stream processing engine that simplifies building data pipelines by enabling you to define them using just SQL. Think of SQLFLow as a lightweight, modern Flink.

## Key Features:

- Process data from Kafka, WebSockets, and more.
- Write ouputs to PostgreSQL, Kafka topics, or cloud storage (such as S3), in a variety of formats, including parquet and iceberg.
- Built on DuckDB and Apache Arrow for high-speed processing.
