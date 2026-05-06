---
title: Streaming databases & dataflow
keywords: [streaming, dataflow, cdc, materialized-views, aqp, delta-live]
status: reviewed
---

# Streaming databases & dataflow

Notes on the **streaming side** of databases: getting changes *out* of an OLTP
store (CDC), maintaining **materialized views as cache** for read-heavy apps
(Noria), running **approximate queries** over unbounded streams (AQP), and
the managed-pipeline products that wrap all of this up (Delta Live Tables).

The upstream side of any streaming-DB pipeline is almost always a message
broker — see [[kafka]], [[redpanda]], [[pulsar]], [[nats]], [[kinesis]], or
[[messaging/mqtt|mqtt]] for those.

## What's in this section

### Articles

- [[cdc]] — Change Data Capture (Flink CDC, Scylla CDC, chgcap, Debezium).
- [[noria]] — MIT/PDOS streaming dataflow that maintains SQL materialized
  views as a cache for web apps.

### Subsections

- [[db/streaming/streaming_query_approximation/README|streaming_query_approximation]] —
  AQP papers (sketches, sampling, wavelets) for one-pass aggregate
  approximations on streams.
- [[db/streaming/delta_live/README|delta_live]] — placeholder for Databricks
  Delta Live Tables notes.

## External entry points

- [Apache Flink](https://flink.apache.org/) — the canonical open-source
  streaming engine.
- [Materialize](https://materialize.com/) — streaming SQL DB built on the
  Timely Dataflow / Differential Dataflow research line.
- [RisingWave](https://risingwave.com/) — PostgreSQL-compatible streaming DB.
- [Debezium](https://debezium.io/) — the de-facto Java CDC stack.
- [Estuary Flow](https://estuary.dev/) — modern hosted CDC + streaming.

## Cross-section see-also

- [[db/analytics/README|analytics]] — the one-shot OLAP counterpart.
- [[kafka]], [[redpanda]], [[pulsar]], [[nats]] — broker side.
- [[messaging/mqtt|mqtt]] — IoT-style ingest.
- [[redis]] — the obvious "just cache it" alternative to streaming
  materialized views.

## Keywords

`#streaming` `#dataflow` `#cdc` `#materialized-views` `#aqp`
