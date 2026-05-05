---
title: parseable
main_link: https://github.com/parseablehq/parseable
keywords: [parseable, data, rust, programming, ingestion, query, compatible, demo]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/programming/rust/data/db.md` by `article_split.py`. Heading: **parseable**.

# parseable

**Main link:** <https://github.com/parseablehq/parseable>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[db]] — diesel _(score 23)_
- [[programming/rust/data/seaquery|seaquery]] — SeaQuery _(score 23)_
- [[trustfall]] — Trustfall _(score 23)_
- [[rtic]] — RTIC _(score 20)_
- [[json]] — JSON _(score 20)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#parseable` `#data` `#rust` `#programming` `#ingestion` `#query` `#compatible` `#demo`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# parseable


https://github.com/parseablehq/parseable
Parseable is a lightweight, cloud native log observability engine. It can use either a local drive or S3 (and compatible stores) for backend data storage.

Parseable is written in Rust and uses Apache Arrow and Parquet as underlying data structures. Additionally, it uses a simple, index-free mechanism to organize and query data allowing low latency, and high throughput ingestion and query.

Parseable consumes up to ~80% lower memory and ~50% lower CPU than Elastic for similar ingestion throughput.

Parseable UI Demo (Credentials: parseable,parseable) ↗︎
Grafana Dashboard Demo ↗︎
🚀 Features
Choose your own storage backend - local drive or S3 (or compatible) object store.
Ingestion API compatible with HTTP + JSON output of log agents - Fluentbit ↗︎, Vector ↗︎, Logstash ↗︎ and others.
Query log data with PostgreSQL compatible SQL.
Grafana ↗︎ for visualization.
Auto schema inference (schema evolution coming soon ↗︎).
Send alerts ↗︎ to webhook targets including Slack.
Stats API ↗︎ to track ingestion and compressed data.
Single binary includes all components - ingestion, store and query. Built-in UI.
