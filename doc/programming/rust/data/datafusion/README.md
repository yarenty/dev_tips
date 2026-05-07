---
title: Apache DataFusion ecosystem (Rust)
keywords: [datafusion, arrow, rust, query-engine, lakehouse, spark-accelerator, columnar]
status: reviewed
---

# Apache DataFusion ecosystem (Rust)

[**Apache DataFusion**](https://datafusion.apache.org/) is the Rust columnar query engine that lives under the Apache Arrow umbrella. It compiles SQL and DataFrame expressions (via [`sqlparser`](https://github.com/apache/datafusion-sqlparser-rs)) into a physical plan over Apache Arrow record batches, with vectorised execution, async I/O, pluggable catalogs, and pluggable storage. Originally built by **Andy Grove** as a personal project (2018), donated to the Apache Software Foundation in 2019, and now maintained by a large community led by **Andrew Lamb** (InfluxData), DataFusion is the embedded engine inside [Ballista](https://datafusion.apache.org/ballista/), [InfluxDB 3.0 (IOx)](https://www.influxdata.com/), [GreptimeDB](https://greptime.com/), [Spice.ai](https://spice.ai/), [Apache Comet](https://datafusion.apache.org/comet/), [Coralogix DataPrime](https://coralogix.com/), GlareDB, Sail, ROAPI, [LakeSoul](https://github.com/lakesoul-io/LakeSoul), and many others.

This subsection is the Rust **code-side** view of the DataFusion ecosystem — sibling projects in the family, accelerators that wrap engines DataFusion competes with, and the lakehouse formats DataFusion has to read. For DataFusion itself as a query engine (gap analysis vs Spark, migration notes, internals), see the analytics-side companion folder [[../../../../db/analytics/datafusion/README|db/analytics/datafusion]].

## What's in this section

| Article | Role | One-liner |
|---|---|---|
| [[blaze]] | Spark accelerator (CPU) | DataFusion-backed Spark plugin from Kuaishou. |
| [[gluten]] | Spark accelerator (CPU) | Apache Gluten — Velox/ClickHouse-backed Spark offload, Substrait IR. |
| [[rapids]] | Spark accelerator (GPU) | NVIDIA's GPU-side Spark plugin via cuDF. |
| [[delta]] | Lake table format | `delta-rs` — Delta Lake from Rust/Python without Spark. |
| [[iceberg]] | Lake table format | `iceberg-rust` — Apache Iceberg SDK + DataFusion `TableProvider`. |
| [[lakesoul]] | Lake table format | LakeSoul — streaming + batch lakehouse, DMetaSoul / China-origin. |
| [[vortex]] | File format | Spiral's compressed Arrow-native columnar format, Parquet successor. |
| [[xorq]] | SQL-on-DataFrames | xorq (formerly LetSQL) — deferred multi-engine ML pipelines on Ibis + DataFusion. |

### Grouped by role

**Engine extensions / Spark accelerators.** [[blaze]], [[gluten]], [[rapids]]. All three plug into Spark and replace JVM-side execution; differ on backend (DataFusion vs Velox/ClickHouse vs NVIDIA cuDF) and CPU-vs-GPU axis. The most "official" DataFusion-on-Spark slot is actually **Apache Comet** (Apple-backed, Apache Incubator) — not yet covered by a dedicated article here; see [[blaze]] and [[gluten]] for the comparison tables.

**Lake table formats.** [[delta]], [[iceberg]], [[lakesoul]]. The "open table format" layer on top of Parquet — ACID transactions, schema evolution, time travel. Iceberg has the broadest cross-engine adoption; Delta has Databricks momentum; LakeSoul is the niche newcomer. See the comparison table in [[delta]] for the picker.

**Storage / file formats.** [[vortex]]. One layer below table formats. Aspires to be a Parquet successor with much faster random access and pluggable modern compression encodings.

**SQL-on-DataFrames.** [[xorq]]. Python deferred-execution dataframe layer on Ibis + DataFusion, with caching and Rust-side ML UDFs. The flagship SAM-on-`candle` demo lives at [[../ml_letsql]].

## DataFusion itself — external entry points

- [datafusion.apache.org](https://datafusion.apache.org/) — project site.
- [github.com/apache/datafusion](https://github.com/apache/datafusion) — main repo.
- [github.com/apache/datafusion-comet](https://github.com/apache/datafusion-comet) — Apache Comet (DataFusion-on-Spark).
- [github.com/apache/datafusion-ballista](https://github.com/apache/datafusion-ballista) — Ballista, distributed DataFusion.
- [Andrew Lamb's blog](https://andrew.nerdnetworks.org/) — engine-internals deep dives.
- [Synnada DataFusion blog](https://datafusion.apache.org/blog/) — release notes and design posts.
- [Apache Arrow](https://arrow.apache.org/) — the columnar memory format underneath everything in this folder.

## See also

- [[../README|programming/rust/data]] — wider Rust data section (drivers, pools, formats).
- [[../../../../db/analytics/datafusion/README|db/analytics/datafusion]] — DataFusion analytics-side notes (Spark-gap analysis, migration, RDD-vs-SQL, concurrent writes).
- [[../../../../db/distributed/ballista_distributed/README|db/distributed/ballista_distributed]] — Ballista, the distributed-DataFusion sibling.
- [[../../../../db/formats/README|db/formats]] — table-format and protocol landing.
- [[../../../../db/catalogs/README|db/catalogs]] — catalog landscape (Hive, REST, Polaris, Nessie).
- [[../ml_letsql]] — LetSQL/xorq + candle SAM UDF deep dive.
- [[../../sql_engine/datafusion|sql_engine/datafusion]] — DataFusion SQL query planner notes.
- [[../../projects/datafusion/README|projects/datafusion]] — DataFusion-projects parking spot (mostly empty).

## Keywords

`#datafusion` `#arrow` `#rust` `#query-engine` `#lakehouse` `#spark-accelerator` `#columnar`
