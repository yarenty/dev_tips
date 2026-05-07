---
title: "SingleStore — distributed SQL with row+columnar HTAP"
main_link: https://www.singlestore.com/
keywords: [singlestore, memsql, distributed-sql, htap, columnar, vector, mysql-compatible]
status: reviewed
---

# SingleStore — distributed SQL with row+columnar HTAP

**Main link:** <https://www.singlestore.com/>

Docs: <https://docs.singlestore.com/> · WASM toolkit tutorial: <https://singlestore-labs.github.io/singlestore-wasm-toolkit/html/Tutorial-Impl-Rust-Split.html>

## Summary

SingleStore (formerly **MemSQL**) is a distributed SQL database that combines an in-memory rowstore (for OLTP-shaped writes) with a disk-based columnstore (for analytics) in the same engine, and presents a MySQL-wire-compatible interface. The marketed sweet spot is **HTAP** — running transactions and analytics on the same data without ETL into a separate warehouse — plus a vector-search column type bolted on more recently.

## Insight

Where SingleStore is a genuinely good fit:

- **Real-time analytics on transactional data.** You're inserting events / orders / clicks at high rate and you want sub-second analytical queries over the same tables, without a separate Snowflake/BigQuery pipeline.
- **You already speak MySQL.** Drivers, BI tools, ORMs all just work.
- **You want a single engine instead of "Postgres + Clickhouse + a CDC pipeline".** That stack is fine and cheap, but operationally heavier than one cluster.
- **Bolted-on vector search** that lives next to your operational data, instead of a separate [[qdrant]] / [[chroma]] cluster.

Where it's the wrong choice:

- **Pure OLTP at small scale** — Postgres or MySQL costs less and works fine.
- **Pure analytics on cold data** — ClickHouse / DuckDB / Snowflake will be cheaper per-query.
- **You don't want a commercial vendor relationship.** SingleStore is source-available with a free tier, but the production story assumes their cloud or a paid licence. Compare with [TiDB](https://www.pingcap.com/tidb/) (open-source HTAP, MySQL-compatible) or [CockroachDB](https://www.cockroachlabs.com/) (open-core, Postgres-compatible).

The MemSQL → SingleStore rebrand happened in 2020; older blog posts and benchmarks still use the old name. There's also the WASM-UDF angle — you can ship Rust/AssemblyScript user-defined functions into the database (the linked tutorial above), which is genuinely novel.

## References / raw notes

- WASM toolkit tutorial (Rust UDFs): <https://singlestore-labs.github.io/singlestore-wasm-toolkit/html/Tutorial-Impl-Rust-Split.html>
- SingleStore as a vector store: <https://docs.singlestore.com/db/v8.5/vector-data/>

## Similar / related topics

- [[postgresql]] — the boring default; pair with Citus for sharding or Timescale for time-series.
- [[mysql]] — wire-protocol cousin (SingleStore speaks the MySQL protocol).
- [TiDB](https://www.pingcap.com/tidb/) — open-source distributed MySQL-compatible HTAP DB.
- [CockroachDB](https://www.cockroachlabs.com/) — distributed SQL, Postgres-wire-compatible.
- [ClickHouse](https://clickhouse.com/) — pure analytical column store.

## Internal links

- [[postgresql]]
- [[mysql]]
- [[qdrant]]

## Keywords

`#singlestore` `#memsql` `#distributed-sql` `#htap` `#columnar` `#vector-search` `#mysql-compatible` `#db`
