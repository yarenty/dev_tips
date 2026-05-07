---
title: "TensorBase — Rust ClickHouse-alternative for analytics (likely dormant)"
main_link: https://github.com/tensorbase/tensorbase
keywords: [tensorbase, clickhouse, rust, arrow, datafusion, analytics, columnar, dormant]
status: reviewed
review_date: 2026/05/03
---

# TensorBase — Rust ClickHouse-alternative for analytics (likely dormant)

**Main link:** <https://github.com/tensorbase/tensorbase>

ClickHouse (the comparison target): <https://clickhouse.com/>

## Summary

TensorBase was an attempt at a modern OLAP / data-warehousing engine written in Rust on top of Apache Arrow and DataFusion, with a ClickHouse-compatible SQL syntax and wire protocol. Its stated goal was to fix the perceived problems of existing big-data systems (low efficiency hidden behind "scalable", hard to use, slow to evolve with modern hardware) by being a single-binary, redesigned columnar store with a top-class network transport.

**Activity warning:** the repo has had little visible activity for years. Treat this article as an archaeological note rather than a recommendation.

## Insight

Why it's interesting historically:

- It was an early example of the "rewrite an existing data-system in Rust on Arrow/DataFusion" pattern that later became common (see [[greptimedb]], InfluxDB 3.x / IOx, [[questdb]] tooling, several lakehouse engines).
- ClickHouse-protocol compatibility was a clever wedge — instead of asking users to relearn a query language, you swap the binary.
- Single-binary, "DBA-free", green-installation philosophy aligns with a clear modern trend.

Why you shouldn't run it today:

- The repo's commit cadence has stalled; issues sit unanswered. A stagnant database is a liability.
- For ClickHouse semantics, just run [ClickHouse](https://clickhouse.com/) — it's mature, well-staffed, and battle-tested.
- For "Rust + Arrow + DataFusion" as a foundation, the active projects to study are [GreptimeDB](https://greptime.com/), [InfluxDB 3](https://www.influxdata.com/), [Apache DataFusion](https://datafusion.apache.org/) directly, and [Polars](https://pola.rs/).

If you're shopping the design space, treat TensorBase as a "what could have been" prototype and look at its successors instead.

## References / raw notes

From the original README:

> TensorBase is a new big data warehousing with modern efforts. Built on top of Rust, Apache Arrow, and Arrow DataFusion. Aims to change the status quo of big-data systems: low efficiency in the name of "scalable", hard to use for end users and understand for developers, not evolving with modern infrastructure.

Stated features:

- Out-of-the-box, lightweight setup.
- Modern columnar storage redesign.
- High-performance network transport server.
- ClickHouse-compatible syntax.
- Green installation, DBA-free ops.
- WIP: HA, clustering, cloud-native adaptation, Arrow data lake.

## Similar / related topics

- [ClickHouse](https://clickhouse.com/) — what to use if you want this niche, today.
- [[db/timeseries/greptimedb|GreptimeDB]] — actively-developed Rust-on-DataFusion time-series / observability DB.
- [Apache DataFusion](https://datafusion.apache.org/) — the query engine TensorBase was built on.
- [Polars](https://pola.rs/) — the in-process Arrow-native dataframe alternative.
- [DuckDB](https://duckdb.org/) — "SQLite for analytics", embedded.

## Internal links

- [[postgresql]]
- [[db/timeseries/greptimedb|GreptimeDB]]
- [[druid]]

## Keywords

`#tensorbase` `#clickhouse-compatible` `#rust` `#arrow` `#datafusion` `#analytics` `#columnar` `#dormant` `#db`
