---
title: GreptimeDB — Rust client / embedding angle
main_link: https://github.com/GreptimeTeam/greptimedb
keywords: [greptimedb, time-series, rust, mysql-protocol, postgres-protocol]
status: reviewed
---

# GreptimeDB — Rust client / embedding angle

**Main link:** <https://github.com/GreptimeTeam/greptimedb>

## Summary

This page covers the **Rust-side angle** for GreptimeDB — the client crate, embedding patterns, and the fact that the server itself is Rust-built — see [[db/timeseries/greptimedb|GreptimeDB (substrate)]] reviewed in P5.S for the operational/server story (architecture, columnar layout, time-series semantics, distributed mode). From a Rust-application perspective GreptimeDB exposes three wire protocols: a native gRPC API (`greptimedb-client` crate), MySQL-compatible, and PostgreSQL-compatible — so most existing Rust SQL clients (`mysql_async`, `tokio-postgres`, `sqlx`) work against it unchanged.

## Insight

For a Rust app needing a time-series store, the easiest on-ramp is to point your existing `tokio-postgres` (or `sqlx` postgres) client at GreptimeDB — no new client library required. Reach for the native `greptimedb-client` gRPC crate when you need the columnar bulk-write path (much faster than INSERT-via-pgwire for high-cardinality metric ingest). Because GreptimeDB is itself Rust + DataFusion + Apache Arrow under the hood, it slots cleanly into the **"Rust single-binary infra rewrite"** trend (alongside Redpanda, OpenObserve, ParseableDB, InfluxDB IOx). Gotchas: the SQL surface is a strict subset (no triggers, no full PG procedural support, time-specific extensions); the project is young — pin versions; the embedded "standalone" mode is for dev only, production is the distributed cluster.

## Similar / related topics

- [[db/timeseries/greptimedb|GreptimeDB (substrate)]] — architecture, columnar layout, distributed mode.
- [[parseable]] — Rust log analytics, same single-binary trend.
- InfluxDB IOx — also Rust + DataFusion + Arrow.
- `tokio-postgres` / `mysql_async` — clients you can use against Greptime out of the box.
- TimescaleDB — Postgres extension comparison point.

## Internal links
- [[db/timeseries/greptimedb|GreptimeDB (substrate)]]
- [[parseable]]
- [[datafusion/README|DataFusion]]
- [[lance_data_format]]

## Keywords

`#greptimedb` `#time-series` `#datafusion` `#mysql-wire` `#postgres-wire`

## References / raw notes

- Repo: <https://github.com/GreptimeTeam/greptimedb>
- Rust client crate: <https://crates.io/crates/greptimedb-client>
- Substrate-side article: [[db/timeseries/greptimedb|db/timeseries/greptimedb]]

GreptimeDB is an open-source time-series database with a focus on scalability, analytical capabilities and efficiency. Designed for cloud-era infrastructure with elastic, commodity storage. Headline features:

- Standalone binary that scales to a highly-available distributed cluster.
- Optimised columnar layout for time-series — compacted, compressed, on multiple storage backends.
- Flexible index options to handle high cardinality.
- Distributed parallel query execution.
- Native SQL plus Python scripting for advanced analytics.
- Wide protocol support: gRPC, MySQL, PostgreSQL, InfluxDB line protocol, Prometheus remote write/read, OpenTSDB.
- Extensible table-engine architecture.
