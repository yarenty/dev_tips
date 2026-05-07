---
title: "QuestDB — high-write-rate time-series database for fintech and IoT"
main_link: https://questdb.io/
keywords: [questdb, timeseries, ilp, sql, financial, tick-data, iot, java, columnar]
status: reviewed
review_date: 2026/05/03
---

# QuestDB — high-write-rate time-series database for fintech and IoT

**Main link:** <https://questdb.io/>

Repo: <https://github.com/questdb/questdb> · Docs: <https://questdb.io/docs/> · Rust client: <https://questdb.io/docs/clients/ingest-rust/>

## Summary

QuestDB is an open-source, column-oriented relational time-series database written in Java (with native off-heap memory, SIMD where possible) and tuned aggressively for **very high write throughput** with low query latency. It speaks SQL with time-series extensions (`SAMPLE BY`, `LATEST ON`, `ASOF JOIN`), exposes Postgres wire protocol for queries, and accepts ingestion via its own InfluxDB Line Protocol (ILP) implementation over TCP/HTTP/UDP.

## Insight

QuestDB's sweet spot is **the write-heavy time-series corner that breaks general-purpose databases**:

- **Financial tick / market data.** Sub-microsecond timestamps, millions of writes per second per node, ASOF joins to align two time series at non-matching timestamps. This is QuestDB's flagship use case; most public benchmarks come from this domain.
- **Industrial / IoT telemetry** at high cadence: sensor data from many devices, each emitting many measurements per second.
- **Real-time analytics dashboards** where you want SQL over both the freshest data and historical bars.

Pragmatic considerations:

- **Postgres wire protocol = tools just work.** Grafana, DBeaver, JDBC, psql — all connect.
- **ILP for ingestion**, SQL for queries. Don't try to write at scale through the Postgres protocol; use the ILP client (Rust, Go, Python, Java, C, Node, .NET).
- **Single-node first.** Replication / clustering is improving but the design centre is "one beefy box that ingests a million points/sec". For multi-node distributed time-series, look at [[db/timeseries/greptimedb|GreptimeDB]] or VictoriaMetrics.
- **License nuance:** open-source core is Apache 2.0; QuestDB Enterprise/Cloud adds commercial features (replication, RBAC).

When QuestDB is **not** the right pick:

- General observability metrics — [[prometheus]] + Mimir/Thanos is the boring default.
- Document or relational workloads — that's a [[postgresql]] job.
- Petabyte historical lakehouse — ClickHouse or a Parquet-on-S3 stack will be cheaper.
- You want the engine itself in Rust (modern Arrow/DataFusion school) — [[db/timeseries/greptimedb|GreptimeDB]] or InfluxDB 3.

The "Java but designed like C" implementation choices make QuestDB an interesting study in low-allocation, off-heap, SIMD-aware JVM code.

## References / raw notes

- Homepage: <https://questdb.io/>
- Docs: <https://questdb.io/docs/>
- Repo: <https://github.com/questdb/questdb>
- Rust client docs: <https://questdb.io/docs/clients/ingest-rust/>
- YouTube demo / overview: <https://www.youtube.com/watch?v=A8uMF64rbS8>

> Fast SQL for time series. QuestDB is the fastest open source time series database. A relational column-oriented database designed for time series and event data. SQL with extensions for time series to assist with real-time analytics.

## Similar / related topics

- [[influxdb]] — older / broader-scoped TSDB; 3.x has overlapping ambitions.
- [[db/timeseries/greptimedb|GreptimeDB]] — Rust + Arrow/DataFusion alternative; multi-node story is stronger.
- [TimescaleDB](https://www.timescale.com/) — Postgres-extension TSDB; the boring default.
- [ClickHouse](https://clickhouse.com/) — column store that's also a credible TSDB at scale.
- kdb+ — proprietary, the historical gold standard for tick data; QuestDB pitches as the open-source answer.

## Internal links

- [[influxdb]]
- [[db/timeseries/greptimedb|GreptimeDB]]
- [[druid]]
- [[postgresql]]

## Keywords

`#questdb` `#timeseries` `#tsdb` `#ilp` `#sql` `#financial` `#tick-data` `#iot` `#columnar` `#db`
