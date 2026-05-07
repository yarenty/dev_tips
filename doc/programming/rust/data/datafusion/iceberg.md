---
title: iceberg-rust — Apache Iceberg from Rust (and DataFusion integration)
main_link: https://github.com/apache/iceberg-rust
keywords: [iceberg, iceberg-rust, datafusion, lakehouse, table-format, apache, parquet, snapshots]
status: reviewed
review_date: 2026/05/03
---

# iceberg-rust — Apache Iceberg from Rust (and DataFusion integration)

**Main link:** <https://github.com/apache/iceberg-rust>

## Summary

**Apache Iceberg** is a high-performance open table format for huge analytic tables: ACID transactions, schema and partition evolution, hidden partitioning, and snapshot-based time travel, layered on top of Parquet (or ORC / Avro) files in object storage. It originated at Netflix in 2018, was donated to the ASF, and is the format engines like Spark, Trino, Flink, Presto, Hive, and Impala can all safely write to concurrently. **`iceberg-rust`** is the official Rust SDK (Apache, in incubation) — including a first-party **DataFusion integration** (`crates/integrations/datafusion/`) that registers an Iceberg table as a `TableProvider`.

## Insight

Iceberg is one of the three contender open-table formats — alongside [[delta|Delta Lake]] and Apache Hudi — and the one with the **broadest cross-engine adoption**: Snowflake, BigQuery, Trino, Flink, ClickHouse, DuckDB, Athena, Redshift, and Spark all read it, and most also write it. Reach for Iceberg (over Delta) when:

- you need **multi-engine writes** with proper concurrency (Iceberg's catalog-mediated commits are more engine-agnostic than Delta's filesystem-mediated commits),
- you want **hidden partitioning** (write `INSERT … timestamp_col`, the table partitions by day automatically; no need to materialise a partition column),
- you're invested in the **Iceberg REST catalog** spec, which has become the de-facto open catalog protocol (Polaris, Tabular, Nessie, Lakekeeper all speak it).

`iceberg-rust` lets non-JVM applications participate in this ecosystem — Rust services, Python (via PyO3), DataFusion-based engines. The DataFusion integration in `crates/integrations/datafusion` is the canonical way to query Iceberg tables from a DataFusion process; `JanKaul/iceberg-rust` is the older, independent Rust implementation that predates the Apache one and is largely superseded.

Gotchas: `iceberg-rust` is **incubating** — feature parity with the Java reference is still catching up (V3 spec features, complex types, position deletes); the **catalog story is still fragmented** (REST is the future, but Hive Metastore, Glue, Nessie, JDBC catalogs all show up in the wild); and "concurrent writes from any engine" is true only when every writer goes through the same catalog and respects the optimistic-concurrency protocol.

## Similar / related topics

- [[delta]] — Delta Lake / `delta-rs`, the main alternative open-table format.
- [[lakesoul]] — LakeSoul, a newer streaming-first lakehouse format.
- **Apache Hudi** — the third major open table format, streaming/upsert bias.
- **Apache Polaris / Lakekeeper / Nessie** — open Iceberg REST catalog implementations.
- [[vortex]] — file format (one layer below table format).

## Internal links

- [[README]] — DataFusion ecosystem landing.
- [[delta]] — sibling open-table format.
- [[lakesoul]] — newer lakehouse format.
- [[../../../../db/formats/README|db/formats]] — broader format landing.
- [[../../../../db/catalogs/README|db/catalogs]] — catalog landscape (REST, Hive, Glue, Polaris, Nessie).
- [[../README|rust/data]] — Rust data section.

## Keywords

`#iceberg` `#iceberg-rust` `#datafusion` `#lakehouse` `#table-format` `#apache` `#parquet` `#snapshots`

## References / raw notes

- Project site: <https://iceberg.apache.org/>
- Java reference: <https://github.com/apache/iceberg>
- Rust SDK: <https://github.com/apache/iceberg-rust>
- DataFusion integration crate: <https://github.com/apache/iceberg-rust/tree/main/crates/integrations/datafusion>
- `TableProviderFactory` source: <https://github.com/apache/iceberg-rust/blob/main/crates/integrations/datafusion/src/table/table_provider_factory.rs>
- Independent earlier Rust impl (now largely superseded): <https://github.com/JanKaul/iceberg-rust>
