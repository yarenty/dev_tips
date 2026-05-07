---
title: CrateDB
main_link: https://crate.io/
keywords: [cratedb, distributed-sql, postgres-wire, lucene, elasticsearch, jvm]
status: reviewed
---

# CrateDB

**Main link:** <https://crate.io/>

## Summary

CrateDB is a distributed SQL database built on top of Lucene + an Elasticsearch-derived shard/cluster layer, with a PostgreSQL wire-compatible front end. It is **written in Java** (not Rust) — filed in this section purely for the "PostgreSQL-wire-compatible distributed SQL DB" angle that overlaps with several Rust-built engines listed nearby. The product targets time-series and operational-analytics workloads (IoT, machine data) where you want SQL plus full-text search plus horizontal scaling without having to glue Elasticsearch to a separate OLAP system.

## Insight

Reach for CrateDB when (a) you genuinely want SQL on top of a Lucene-shaped inverted index and you also want JOINs / aggregations across shards, and (b) you're OK with a JVM operational footprint. The PostgreSQL wire protocol means most Postgres clients (`psql`, `pgcli`, JDBC, the Rust `tokio-postgres` / `sqlx` crates, …) "just work" against it — that's the main reason the Rust ecosystem talks about it at all. Versus the obvious alternatives: **YugabyteDB** / **CockroachDB** for distributed OLTP-shaped Postgres-compatible SQL; **TiDB** / **Citus** for sharded MySQL/Postgres; **ClickHouse** / **Apache Pinot** / **Druid** when you don't need search and want pure OLAP; **OpenSearch** / **Elasticsearch** when you want search and would rather skip the SQL pretense. CrateDB has a "Community Edition" plus a commercial Enterprise tier — read the licence carefully if you self-host. UDFs (the original reason this article existed) are JavaScript-based and fairly limited compared to anything in this section's Rust-centric UDF stories.

## Similar / related topics

- **YugabyteDB** — distributed SQL, Postgres-compatible, written in C++ with a Postgres query layer.
- **CockroachDB** — distributed SQL, Postgres-compatible, Go.
- **TiDB** — MySQL-compatible distributed SQL on top of TiKV (Rust storage layer).
- **ClickHouse** — columnar OLAP DB; faster than CrateDB for pure analytics, no full-text search.
- **OpenSearch / Elasticsearch** — the inverted-index engines CrateDB is layered on; closer to the search side of the same trade-off.

## Internal links

<!-- reviewed -->
- [[README]]
- [[databend]] — the Rust-built Snowflake-shaped warehouse in this section.
- [[snowflake]] — managed warehouse comparison.
- [[../../../db/relational/postgresql|PostgreSQL]] — the wire protocol CrateDB speaks.
- [[../../../db/distributed/README|distributed DBs]] — broader landscape.

## Keywords

`#cratedb` `#distributed-sql` `#postgres-wire` `#lucene` `#elasticsearch` `#jvm`

## References / raw notes

- Project home: <https://crate.io/>
- Docs: <https://crate.io/docs/>
- UDF reference (originally linked here): <https://crate.io/docs/crate/reference/en/5.0/general/user-defined-functions.html>
- Source: <https://github.com/crate/crate>
- CrateDB Cloud (managed): <https://console.cratedb.cloud/>
