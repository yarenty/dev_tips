---
title: SQLFlow
main_link: https://github.com/turbolytics/sql-flow
keywords: [sqlflow, streaming-sql, kafka, duckdb, arrow, flink-alternative, materialize, risingwave]
status: reviewed
---

# SQLFlow

**Main link:** <https://github.com/turbolytics/sql-flow>

## Summary

SQLFlow (`turbolytics/sql-flow`) is a lightweight stream-processing engine
that lets you define streaming pipelines in **plain SQL**. It reads from
Kafka, WebSockets, or other sources, runs SQL transformations using **DuckDB
+ Apache Arrow** under the hood, and writes results out to PostgreSQL, Kafka
topics, or cloud object storage (S3 in Parquet or Iceberg). Think of it as
a much simpler, single-binary Flink for the "I want streaming SQL but not a
JVM cluster" tier.

## Insight

Where SQLFlow fits versus the streaming-SQL incumbents:

- **vs Apache Flink SQL** — Flink is the canonical, battle-tested choice for
  large-scale streaming SQL. It also requires a JVM cluster, a job manager,
  and a non-trivial ops investment. SQLFlow is one binary, DuckDB-fast for
  the data sizes that fit it, and trivial to run locally.
- **vs RisingWave** — RisingWave is a streaming database (materialised views
  over streams, persistent state, high-availability story). SQLFlow is a
  *stateless* stream processor — closer to Kafka Streams in spirit. Different
  problem class.
- **vs Materialize** — same shape comparison as RisingWave: Materialize is a
  full streaming database; SQLFlow is a pipeline runtime.
- **vs Kafka Streams** — comparable in scope (consume from Kafka, transform,
  produce to Kafka), but SQL-defined rather than Java/Scala-coded, and with
  DuckDB-class analytical SQL instead of Kafka Streams' DSL.

When to reach for it: you have Kafka topics and want to do non-trivial SQL
transformations into Parquet/Iceberg/Postgres without standing up a Flink
cluster, and your throughput fits a single DuckDB process. The DuckDB +
Arrow backbone means you also get Iceberg writes and Parquet output for free.

When *not* to: you need exactly-once semantics over partitioned state, very
high throughput across many nodes, or persistent materialised views.

## Key features

- **Sources**: Kafka, WebSockets, more.
- **Sinks**: PostgreSQL, Kafka topics, cloud storage (S3 etc.), in formats
  including Parquet and Iceberg.
- **Engine**: DuckDB + Apache Arrow — fast columnar analytical SQL.
- **Footprint**: lightweight; designed to be a "modern, stripped-down Flink".

## Similar / related topics

- **Apache Flink SQL** — the heavyweight incumbent.
- **RisingWave** — streaming database with materialised views.
- **Materialize** — streaming database, similar shape to RisingWave.
- **Apache Kafka Streams** — JVM-only stream processor; comparable scope.
- **DuckDB** — the analytical SQL engine SQLFlow embeds.
- **Apache Iceberg** — output table format option.

## Internal links

<!-- reviewed -->

- [[db/tools/README|tools]] — section landing.
- [[db/streaming/README|streaming]] — broader streaming context.
- [[kafka]] — primary input source.
- [[redpanda]] — Kafka-API-compatible alternative input.
- [[quary]] — sibling SQL-as-code tool (batch instead of streaming).

## Keywords

`#sqlflow` `#streaming-sql` `#kafka` `#duckdb` `#arrow` `#flink-alternative` `#materialize` `#risingwave`

## References / raw notes

- Repo: <https://github.com/turbolytics/sql-flow>

> SQLFlow: DuckDB for Streaming Data. A high-performance stream processing
> engine that simplifies building data pipelines by enabling you to define
> them using just SQL. Think of SQLFlow as a lightweight, modern Flink.
