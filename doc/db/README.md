---
title: Databases
keywords: [databases, db, data, storage, sql, nosql, analytics, streaming]
status: reviewed
---

# Databases

The `db/` subtree is the umbrella for everything storage- and query-shaped:
engines, file/table formats, catalogs, quality, transformation, distribution,
and the operational tooling around them. The grouping below is by *purpose*
rather than alphabetical, so a question like "I have time-series data, where
do I look?" routes to one place.

## Decision-tree shortcut

- **"I need to store rows and run SQL on them"** → [[db/relational/README|relational]]
  (think [[postgresql]], [[mysql]], [[singlestore]], [[edgedb]]).
- **"I need a key/value, document, or wide-column store"** → [[db/nosql/README|NoSQL]]
  (think [[redis]], [[hbase]], [[fauna]], [[firebase]]).
- **"I need similarity search over embeddings"** → [[db/vector/README|vector]]
  (think [[chroma]], [[db/vector/qdrant|Qdrant]]).
- **"I have timestamps and metrics"** → [[db/timeseries/README|time series]]
  (think [[influxdb]], [[questdb]], [[druid]], [[db/timeseries/greptimedb|GreptimeDB]]).
- **"I want OLAP / lakehouse / warehouse-style queries"** → [[db/analytics/README|analytics]].
- **"I need to ship data continuously between systems"** → [[db/streaming/README|streaming]]
  + see also [[kafka]], [[redpanda]], [[pulsar]], [[nats]], [[kinesis]], [[cdc]].
- **"My single node won't fit it / I need replication and sharding"**
  → [[db/distributed/README|distributed]].
- **"I need governance, lineage, discovery, multi-engine metadata"**
  → [[db/catalogs/README|catalogs]].
- **"What file or table format should I write?"** → [[db/formats/README|formats]].
- **"How do I trust this data?"** → [[db/quality/README|quality]].
- **"I need to call non-SQL code from SQL"** → [[db/udf/README|UDF]].
- **"I just want a CLI / utility for some db chore"** → [[db/tools/README|tools]].
- **"What is *data fabric* and do I want one?"** → [[db/fabric/README|fabric]].

## Engines & databases

- [[db/relational/README|relational]] — classical row-stores and OLTP engines.
- [[db/nosql/README|NoSQL]] — KV, document, wide-column, graph.
- [[db/vector/README|vector]] — ANN / similarity search backends.
- [[db/timeseries/README|time series]] — append-mostly, time-indexed workloads.
- [[db/analytics/README|analytics]] — columnar, OLAP, lakehouse, warehouse.

## Movement, change, distribution

- [[db/streaming/README|streaming]] — streaming SQL engines, change feeds,
  approximate query processing.
- [[db/distributed/README|distributed]] — sharding, replication, consensus,
  multi-region designs.

## Schema, format, governance, quality

- [[db/catalogs/README|catalogs]] — metadata layers (Unity, Hive Metastore,
  Glue, Atlas, Polaris, Iceberg REST).
- [[db/formats/README|formats]] — Parquet/Arrow/ORC/Avro on disk;
  Iceberg/Delta/Hudi as tables; Arrow Flight on the wire.
- [[db/quality/README|quality]] — validation, profiling, lineage, and
  AI-shaped data problems.
- [[db/udf/README|UDF]] — user-defined functions, including external/sandboxed.

## Operational tooling & cross-cutting

- [[db/tools/README|tools]] — db-adjacent CLIs: indexing references,
  SQL-as-code transformation, lineage, pentest.
- [[db/fabric/README|fabric]] — the vendor-coined "data fabric" umbrella;
  honest definitions live here.

## See also (cross-section)

- Messaging substrates that databases consume from / publish to:
  [[kafka]], [[redpanda]], [[pulsar]], [[nats]], [[messaging/mqtt|mqtt]].
- Observability for the data plane: [[prometheus]], [[observability/grafana|grafana]],
  [[node_exporter]], [[openobserve]].
- Backup / ops: [[barman]] for Postgres.
- Embedded / single-file / experimental engines: [[limbo]], [[toydb]],
  [[xlite]], [[tensorbase]], [[surrealdb]], [[noria]].

## Keywords

`#db` `#databases` `#data` `#storage` `#sql` `#nosql` `#analytics` `#streaming` `#governance`
