---
title: Analytics databases & engines
keywords: [analytics, olap, query-engine, datafusion, databend, gluten, tensor-query]
status: reviewed
review_date: 2026/05/03
---

# Analytics databases & engines

Notes, decks, and papers I've collected while comparing OLAP / analytics
engines — both production-oriented (Databend, DataFusion, Gluten) and
research-grade (tensor query processing). The READMEs frame what each
subfolder is about, with external pointers to the canonical projects.

## What's in this section

### Engines

- [[db/analytics/databend/README|Databend]] — Rust-native cloud data warehouse;
  benchmark notes vs Druid and SparkSQL.
- [[db/analytics/datafusion/README|DataFusion]] — Apache Arrow's Rust query
  engine; concurrent writes, Ballista gaps, migration notes.
- [[db/analytics/gluten/README|Gluten]] — Spark SQL operator offload to native
  engines (Velox / ClickHouse); Intel OAP project context.

### Research

- [[db/analytics/tensor_query/README|Tensor query processing]] — academic line
  of work on running SQL queries as tensor programs on GPUs (Microsoft +
  Politecnico Milano).

## External entry points

- [Apache DataFusion](https://datafusion.apache.org/)
- [Databend](https://databend.com/)
- [Apache Gluten](https://gluten.apache.org/)
- [DuckDB](https://duckdb.org/) — the other obvious modern OLAP engine
- [ClickHouse](https://clickhouse.com/)

## Cross-section see-also

- [[db/distributed/README|distributed]] — distributed query execution, schedulers,
  Ballista (the distributed runtime for DataFusion).
- [[db/streaming/README|streaming]] — streaming dataflow, CDC, AQP — the
  continuous-query counterpart to one-shot OLAP.
- [[druid]] — referenced in the Databend benchmark decks.

## Keywords

`#analytics` `#olap` `#query-engine` `#datafusion` `#databend` `#gluten` `#tensor-query`
