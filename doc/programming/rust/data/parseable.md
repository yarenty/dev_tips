---
title: Parseable — Rust log analytics on object storage
main_link: https://github.com/parseablehq/parseable
keywords: [parseable, logs, observability, arrow, parquet, s3]
status: reviewed
review_date: 2026/05/03
---

# Parseable — Rust log analytics on object storage

**Main link:** <https://github.com/parseablehq/parseable>

## Summary

Parseable is an open-source, Rust-built log observability engine that uses **Apache Arrow + Parquet** as its on-disk format and supports either local disk or S3 / S3-compatible object storage as the backend. It exposes an HTTP+JSON ingestion API compatible with Fluent Bit, Vector, Logstash, and friends; queries via PostgreSQL-wire-compatible SQL; ships a built-in UI and Grafana datasource. Single binary contains ingestion, store, and query.

## Insight

Parseable is squarely in the **"Rust single-binary infra rewrite"** trend (alongside Redpanda for Kafka, OpenObserve for the ELK stack, GreptimeDB for time-series, InfluxDB IOx, QuestDB). The pitch vs Elasticsearch / Loki: ~50% lower CPU and ~80% lower memory at comparable ingest, single binary instead of a JVM cluster, and S3-as-source-of-truth means you can scale storage cheaply (no replicated hot tier). It's *intentionally* index-free — relies on columnar Parquet + predicate pushdown for query speed rather than per-token inverted indexes — which is fast for filtered/aggregated queries and slow for "find the one error message containing ‹substring›" full-text needles. Pair with Grafana for dashboards. Gotchas: schema evolution is auto but rough at the edges (rename ≈ new column); object-storage latency dominates cold queries; the project is young — pin versions.

## Similar / related topics

- OpenObserve — closest peer; also Rust, also S3-backed, more "ELK + Loki + Mimir + Tempo replacement" framing.
- Loki / Grafana — log store widely deployed alongside Prometheus.
- Quickwit — Rust log search optimised for object storage; closer to Elasticsearch on the search side.
- ClickHouse — heavyweight OLAP that many teams use as a log backend.
- Elasticsearch / OpenSearch — the JVM incumbents.

## Internal links
- [[../../../observability/openobserve|OpenObserve]]
- [[../../../observability/grafana|Grafana]]
- [[lance_data_format]] — Arrow-native columnar format
- [[meilisearch]] — different niche (full-text, not logs)
- [[greptimedb]] — sibling Rust single-binary

## Keywords

`#parseable` `#logs` `#observability` `#arrow` `#parquet` `#s3`

## References / raw notes

- Repo: <https://github.com/parseablehq/parseable>
- Docs: <https://www.parseable.com/docs>

> Parseable is a lightweight, cloud-native log observability engine. Local drive or S3 (and compatible) for backend storage. Written in Rust; uses Apache Arrow + Parquet as underlying data structures; index-free organisation for low-latency, high-throughput ingest and query.

Headline features:

- Storage: local drive **or** S3-compatible object store.
- Ingestion API: HTTP + JSON, compatible with Fluent Bit, Vector, Logstash.
- Query: PostgreSQL-compatible SQL.
- Visualisation: Grafana datasource included.
- Auto schema inference (schema evolution still maturing).
- Alerts to webhook targets including Slack.
- Stats API for ingestion / compressed-size accounting.
- Single binary: ingest + store + query + UI.
