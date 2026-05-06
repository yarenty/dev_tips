---
title: Databend
keywords: [databend, olap, rust, data-warehouse, analytics, benchmark]
status: reviewed
---

# Databend

[Databend](https://databend.com/) is an open-source, Rust-native cloud data
warehouse designed as a Snowflake alternative. It separates compute from
storage (object storage as the source of truth), speaks SQL, and is built on
top of Apache Arrow.

Mostly worth knowing as one of the modern Rust-rewritten data-warehouse
options, alongside [[db/timeseries/greptimedb|GreptimeDB]], [QuestDB](https://questdb.io/),
and the InfluxDB 3 (IOx) lineage.

## External entry points

- [databend.com](https://databend.com/)
- [github.com/datafuselabs/databend](https://github.com/datafuselabs/databend)
- [Databend docs](https://docs.databend.com/)

## Cross-section see-also

- [[db/analytics/datafusion/README|DataFusion]] — same Arrow/Rust ecosystem;
  Databend reuses pieces of it.
- [[db/analytics/README|analytics]] — siblings (DataFusion, Gluten, tensor query).
- [[druid]] — one of the comparison engines.

## Keywords

`#databend` `#olap` `#rust` `#data-warehouse` `#benchmark`
