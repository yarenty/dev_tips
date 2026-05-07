---
title: Distributed databases & query execution
keywords: [distributed, ballista, federated-query, scheduling, long-running-query]
status: reviewed
review_date: 2026/05/03
---

# Distributed databases & query execution

Notes, decks, and papers on the **distributed side** of databases:
distributing query execution across nodes (Ballista, federated execution),
scheduling distributed work fairly and efficiently, and handling
long-running queries with checkpointing and fault tolerance.

Most subfolders are asset-only — paper PDFs, design decks, screenshot
captures from whiteboard sessions — with a README that frames the topic and
points at the canonical external references.

## What's in this section

### Engines & execution

- [[db/distributed/ballista_distributed/README|ballista_distributed]] —
  Apache Ballista, the distributed execution engine on top of DataFusion.
- [[db/distributed/federated_query_execution/README|federated_query_execution]] —
  whiteboard / screenshot working notes on federated query execution.
- [[db/distributed/federated/README|federated]] — placeholder for federated
  query systems (Trino / Presto / Calcite / FedML).
- [[db/distributed/long_running_query/README|long_running_query]] — notes on
  handling queries that run for hours (checkpointing, restart, fault
  tolerance).

### Scheduling

- [[db/distributed/scheduling/README|scheduling]] — top of the
  cluster-/job-scheduler subtree (Google, Microsoft, VLDB papers).

## External entry points

- [Apache Ballista](https://github.com/apache/datafusion-ballista) —
  Rust/Arrow distributed engine.
- [Trino](https://trino.io/) / [Presto](https://prestodb.io/) — canonical
  federated SQL query engines.
- [Apache Calcite](https://calcite.apache.org/) — SQL parser / relational
  algebra / federation toolkit.
- [Apache Spark](https://spark.apache.org/) — the elephant in the room.

## Cross-section see-also

- [[db/analytics/datafusion/README|DataFusion]] — Ballista is the
  distributed runtime for DataFusion.
- [[db/analytics/README|analytics]] — single-node OLAP engines.
- [[db/streaming/README|streaming]] — distributed streaming dataflow
  (Materialize, RisingWave, Flink).

## Keywords

`#distributed` `#ballista` `#federated-query` `#scheduling` `#query-execution`
