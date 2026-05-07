---
title: "GreptimeDB — Rust hybrid time-series + observability database"
main_link: https://greptime.com/
keywords: [greptimedb, timeseries, observability, rust, datafusion, arrow, parquet, single-binary]
status: reviewed
---

# GreptimeDB — Rust hybrid time-series + observability database

**Main link:** <https://greptime.com/>

Repo: <https://github.com/GreptimeTeam/greptimedb> · Docs: <https://docs.greptime.com/>

## Summary

GreptimeDB is an open-source distributed time-series database written in Rust on top of Apache Arrow / DataFusion / Parquet, with object-storage-first architecture (S3, GCS, Azure Blob, OSS) and a hybrid query model that aims to unify **metrics, logs, and events** in a single engine. It speaks SQL and PromQL, supports MySQL and Postgres wire protocols for queries, and ingests via Prometheus remote-write, InfluxDB line protocol, OpenTelemetry, and its own gRPC.

A single binary handles standalone, distributed, and edge deployments. The project is part of the broader "Rust single-binary infra" trend that includes [[redpanda]] (Kafka-replacement) and [[openobserve]] (observability stack).

## Insight

Where GreptimeDB is interesting today:

- **You want one engine for metrics + logs + traces** instead of Prometheus + Loki + Tempo + glue. GreptimeDB pitches itself as that engine, with a story for each.
- **You want cheap retention via object storage.** Hot data on local disk, cold data in S3, transparent to queries.
- **You like the "Rust + Arrow + DataFusion + Parquet" architectural school** that also produced InfluxDB 3, [[tensorbase]] (dormant), and is the backbone of much modern data infra.
- **You want PromQL and SQL on the same data.** Useful for teams that have grown a Prometheus dialect on the SRE side and want SQL for product analytics on the same metrics.

Honest caveats:

- **Younger than the alternatives.** The 1.0 release was relatively recent; the road to "battle-tested at petabyte scale" is shorter than Prometheus + Mimir or InfluxDB.
- **Multi-node distributed story is the strategic differentiator** but requires care to operate (metasrv + frontend + datanode roles, Etcd/PostgreSQL for metadata).
- **vs InfluxDB 3** — very similar architectural school. Pick GreptimeDB if you want a single engine for metrics + logs and like its multi-protocol ingestion. Pick InfluxDB 3 if you want the Influx ecosystem and managed cloud product. Both are Rust + Arrow + DataFusion underneath.
- **vs ClickHouse + VictoriaMetrics + Loki** — the unbundled stack is more mature and more flexible. GreptimeDB's bet is that the bundled stack is cheaper to operate and runs faster end-to-end.

## Note on the sibling article

There's also `programming/rust/data/greptimedb.md` in this vault, which covers GreptimeDB from the Rust-ecosystem perspective. This article is the database-perspective one. Use the disambiguated wikilink `[[db/timeseries/greptimedb|GreptimeDB]]` when linking to it.

## References / raw notes

> GreptimeDB is an open-source time-series database with a special focus on scalability, analytical capabilities and efficiency. It's designed to work on infrastructure of the cloud era, and users benefit from its elasticity and commodity storage.

Stated capabilities:

- Single-binary that scales from standalone to highly-available distributed cluster, transparently to clients.
- Optimised columnar layout for time-series data; compacted, compressed, stored on local disk or various object-storage backends.
- Flexible index options (skipping indexes, inverted indexes for tag cardinality control).
- Distributed, parallel query execution with elastic compute.
- Native SQL plus Python scripting for advanced analytical scenarios.
- Widely-adopted ingestion protocols: Prometheus remote-write, InfluxDB Line Protocol, OpenTelemetry, gRPC, MySQL/Postgres wire for queries.
- Extensible table-engine architecture for varied workloads (metrics, logs, traces, events).

## Similar / related topics

- [[influxdb]] — InfluxDB 3 (IOx) is the closest architectural cousin (Rust + Arrow + DataFusion).
- [[questdb]] — single-node high-write-rate alternative.
- [[druid]] — older OLAP-on-time-bucketed-events alternative.
- [VictoriaMetrics](https://victoriametrics.com/) — best-in-class Prometheus long-term storage; more focused.
- [[openobserve]] — Rust observability stack with overlapping ambitions.

## Internal links

- [[influxdb]]
- [[questdb]]
- [[druid]]
- [[prometheus]]
- [[openobserve]]

## Keywords

`#greptimedb` `#timeseries` `#observability` `#rust` `#datafusion` `#arrow` `#parquet` `#promql` `#sql` `#db`
