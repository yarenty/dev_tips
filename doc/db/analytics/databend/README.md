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

This folder holds early evaluation material from when we benchmarked Databend
against [[druid]] and SparkSQL on single-node workloads — both NCE and
TPC-DS-style.

## What's in this folder

- `Databend_quick_v01.pptx` — short intro deck (architecture, SQL surface,
  storage model).
- `Druid, SparkSQL & Databend NCE Single-Node_translated.docx` — single-node
  benchmark write-up against Druid and SparkSQL on an NCE workload.
- `Druid, SparkSQL & Databend tpcds Single-Node_translated.docx` — single-node
  TPC-DS benchmark numbers across the same three engines.

## External entry points

- [databend.com](https://databend.com/)
- [github.com/datafuselabs/databend](https://github.com/datafuselabs/databend)
- [Databend docs](https://docs.databend.com/)

## Cross-section see-also

- [[db/analytics/datafusion/README|DataFusion]] — same Arrow/Rust ecosystem;
  Databend reuses pieces of it.
- [[db/analytics/README|analytics]] — siblings (Gluten, Tachyons, tensor query).
- [[druid]] — one of the comparison engines.

## Keywords

`#databend` `#olap` `#rust` `#data-warehouse` `#benchmark`
