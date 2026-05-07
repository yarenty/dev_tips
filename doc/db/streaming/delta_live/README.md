---
title: Delta Live Tables
keywords: [delta-live-tables, dlt, databricks, etl, streaming, lakehouse]
status: reviewed
review_date: 2026/05/03
---

# Delta Live Tables

[Delta Live Tables](https://docs.databricks.com/en/delta-live-tables/index.html)
(DLT) is Databricks' managed, declarative ETL framework on top of Delta Lake
and Spark Structured Streaming. You define datasets and dependencies in
Python or SQL; DLT handles orchestration, incremental processing, retries,
checkpointing, schema enforcement, data-quality "expectations", and lineage.

This folder is a **placeholder** for future DLT notes.

## External entry points

- [Delta Live Tables docs](https://docs.databricks.com/en/delta-live-tables/index.html)
- [Delta Lake](https://delta.io/) — the underlying storage format.
- [Apache Spark Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) —
  the engine DLT runs on.

## Cross-section see-also

- [[db/streaming/README|streaming]] — sibling streaming topics (CDC, Noria,
  AQP).
- [[db/streaming/cdc|cdc]] — DLT often consumes CDC feeds as bronze-layer
  inputs.
- [[programming/rust/data/datafusion/delta|delta]] — Rust-side notes on the
  Delta Lake format.

## Keywords

`#delta-live-tables` `#dlt` `#databricks` `#etl` `#lakehouse`
