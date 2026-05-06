---
title: "Time-series databases"
keywords: [timeseries, metrics, observability, prometheus, influxdb, questdb, druid, greptimedb]
status: reviewed
---

# Time-series databases

Landing page for time-series-shaped data stores. Three different shapes of "time-series" hide under one label, and each has a different right answer.

## Decision tree

- **Operational metrics for an SRE / observability stack** (CPU, latency histograms, scrape-style monitoring) → [[prometheus]] (covered under `observability/`). Long-term storage: Thanos, Mimir, VictoriaMetrics. Don't overthink this.
- **Application time-series** (sensor data, business events with timestamps, "what was the value at time T?") → [[influxdb]], [[questdb]], [[db/timeseries/greptimedb|GreptimeDB]], or **TimescaleDB** (Postgres extension — preferred if you're already on [[postgresql]]).
- **OLAP on time-bucketed events** at meaningful scale (clickstream, ad analytics, real-time dashboards over millions of events/sec) → [[druid]], ClickHouse, Apache Pinot.
- **Financial tick / market data with brutal write rates** → [[questdb]] is the niche specialist; kdb+ if you have the budget.
- **Hybrid time-series + observability + traces in one engine** → [[db/timeseries/greptimedb|GreptimeDB]] is the new entrant betting on this convergence.

## Articles

- [InfluxDB](influxdb.md) — the original "DB for metrics"; multiple major versions with very different internals (1.x → 2.x → 3.x / IOx).
- [QuestDB](questdb.md) — Java engine optimised for very high write rates; popular in fintech and IoT.
- [GreptimeDB](greptimedb.md) — Rust hybrid time-series + observability + log DB.
- [Druid](druid.md) — Apache Druid; real-time OLAP on time-bucketed events.

## See also

- `observability/` — Prometheus, Grafana, OpenObserve, the "metrics first" stack.
- `db/analytics/` — ClickHouse, DataFusion, the "columnar OLAP" overlap.
- `db/relational/postgresql.md` — TimescaleDB lives there.

## Internal links

<!-- reviewed -->

- [[influxdb]]
- [[questdb]]
- [[db/timeseries/greptimedb|GreptimeDB]]
- [[druid]]
- [[prometheus]]

## Keywords

`#timeseries` `#metrics` `#observability` `#olap` `#columnar` `#tsdb` `#db`
