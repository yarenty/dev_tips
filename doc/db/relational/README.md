---
title: "Relational databases"
keywords: [relational, sql, postgresql, mysql, oltp, embedded, htap, newsql]
status: reviewed
---

# Relational databases

Landing page for relational / SQL-shaped databases. Covers classic OLTP, the new wave of "serverless" / managed SQL, embedded engines, and a few exotic multi-model systems that still expose a SQL-ish surface.

## How to pick one

A rough decision tree:

- **You need a boring, proven OLTP database.** Use [[postgresql]]. If you live in the WordPress / cPanel / AWS Aurora MySQL ecosystem, use [[mysql]] (or MariaDB).
- **You want SQL but as a managed cloud product.** Look at [[singlestore]] (HTAP), [[edgedb]] (now Gel — Postgres + a typed query language), or [[fauna]] (note: Fauna's hosted service was sunset in 2025).
- **You want something embedded / single-process.** [[limbo]] (Rust SQLite-compatible rewrite from Turso) or plain SQLite. [[xlite]] reads spreadsheets as SQLite virtual tables.
- **You want to learn how a database is built.** Read [[toydb]] — Erik Grinaker's educational distributed SQL DB in Rust. Don't run it in prod.
- **You want analytics on top of SQL.** [[tensorbase]] (ClickHouse-compatible, Rust, Arrow/DataFusion-based — repo activity has stalled) or jump to `db/analytics/` for Clickhouse / DuckDB / DataFusion.
- **You want one database that's also a graph + document + KV store.** [[surrealdb]] — slick pitch, watch maturity.

## Subsections

- [Postgres ecosystem](postgres/README.md) — Postgres-adjacent tools (backups, extensions, ops).

## Articles

### Classic OLTP

- [postgresql](postgresql.md) — the canonical Postgres article, with the "9 things that bite you in production" piece.
- [mysql](mysql.md) — operational notes plus an honest "when MySQL beats Postgres" framing.

### Serverless / managed / NewSQL

- [edgeDB](edgedb.md) — Postgres + an opinionated typed schema/query language (now rebranded **Gel**).
- [Fauna](fauna.md) — serverless multi-region transactional DB (managed service sunset April 2025).
- [Singlestore](singlestore.md) — distributed SQL with row+columnar hybrid storage, aimed at HTAP.

### Embedded / educational

- [Limbo](limbo.md) — Turso's SQLite-compatible rewrite in Rust.
- [toyDB](toydb.md) — educational distributed SQL in Rust.
- [XLite](xlite.md) — query Excel / ODS spreadsheets as SQLite virtual tables.

### Multi-model / exotic

- [SurrealDB](surrealdb.md) — document + graph + relational + KV in a single Rust binary.
- [TensorBase](tensorbase.md) — Rust-based ClickHouse-compatible analytics DB (likely dormant).

## See also

- `db/distributed/` — when one node is no longer enough.
- `db/analytics/` — column stores, OLAP engines, lakehouse query engines.
- `db/vector/` — vector search (or just use `pgvector` on Postgres).
- `db/timeseries/` — time-series-shaped workloads.

## Internal links

- [[postgresql]] — start here for any new project.
- [[mysql]] — the other one.
- [[surrealdb]] — multi-model curiosity.
- [[limbo]] — embedded Rust SQLite.
- [[barman]] — Postgres backup/recovery tool.

## Keywords

`#relational` `#sql` `#oltp` `#postgresql` `#mysql` `#newsql` `#htap` `#embedded` `#db`
