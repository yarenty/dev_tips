---
title: Rust data — drivers, pools, engines, formats
keywords: [rust, data, database, driver, pool, sql, cache, format]
status: reviewed
---

# Rust data — drivers, pools, engines, formats

The `programming/rust/data/` section gathers Rust crates that touch databases, query engines, caches, and data formats. For database substrates themselves (PostgreSQL, MySQL, SurrealDB the server, GreptimeDB the server) see [[../../../db/README|db/]]; for the DataFusion family of analytics engines see the dedicated subsection.

## Subsections

- [[datafusion/README|datafusion/]] — Apache DataFusion + Comet / Blaze / Iceberg / Vortex / Delta / LakeSoul ecosystem.

## Decision shortcut

| You need… | Reach for | Notes |
|---|---|---|
| Async DB pool (Postgres / Redis / Lapin / SQLite) | [[deadpool]] | Modern default, multi-runtime |
| Async DB pool, smaller surface, Tokio-only | [[bb8]] | If you already know `r2d2` |
| Sync DB pool (classic Diesel, rusqlite, sync postgres) | [[r2d2]] | The historical default |
| Talk to MySQL from sync Rust | [[mysql]] | Pure-Rust; for async Rust prefer `sqlx` |
| Talk to MS SQL Server | [[tiberius]] | The only good Rust MSSQL story |
| Talk to SQLite | [[rusqlite]] | + `r2d2-sqlite` for pooling |
| Build SQL queries dynamically | [[seaquery]] | Pair with `sqlx` for execution |
| Run SQL migrations | [[refinery]] | Or `barrel` if you want a Rust DSL |
| Generate schema SQL from Rust | [[barrel]] | Multi-backend; pair with refinery to apply |
| Parse / inspect / rewrite SQL | [[sqlparser]] | The canonical Rust SQL parser |
| Embedded SQL engine, pluggable storage, WASM | [[gluesql]] | "SQLite for weird storage" |
| Embed SurrealDB in a Rust app | [[surrealdb]] | + see [[../../../db/relational/surrealdb|substrate article]] |
| Time-series storage + Rust client | [[greptimedb]] | + see [[../../../db/timeseries/greptimedb|substrate]] |
| Self-host log analytics in Rust | [[parseable]] | Single-binary, Arrow+Parquet, S3 backend |
| Self-host full-text search in Rust | [[meilisearch]] | Algolia-shaped; vs Elasticsearch / Tantivy |
| Query arbitrary heterogeneous data | [[trustfall]] | "GraphQL for anything" |
| In-process cache (TTL, async, eviction) | [[cache]] | Moka |
| Concurrent hash-map | [[map_cache_storage]] | DashMap; or evmap / moka |
| Lock-free read-mostly map | [[memory_storage]] | evmap |
| Speed up Rust / C++ / CUDA builds | [[compilation_cache]] | sccache (S3-shared in CI) |
| Modern columnar format optimised for ML | [[lance_data_format]] | Parquet's challenger |
| Arrow-native columnar JDBC successor | [[adbc]] | + see [[../../../db/formats/table_transfer_protocols/README|protocols]] |
| ML inference inside SQL UDFs | [[ml_letsql]] | LetSQL/xorq + candle demo |
| Crypto primitives (POLYVAL, AES-GCM-SIV) | [[crypto]] | RustCrypto landscape |

## Articles by theme

### DBs and query engines

- [[gluesql]] — embedded SQL engine, pluggable storage, runs in WASM.
- [[surrealdb]] — Rust SDK for SurrealDB (server, embedded, or remote).
- [[greptimedb]] — Rust client + embedding angle for the time-series DB.
- [[parseable]] — Rust log analytics on Arrow + Parquet, S3-backed.
- [[meilisearch]] — Rust full-text search engine.
- [[trustfall]] — query engine for any data source, GraphQL-shaped.
- [[wooridb]] — experimental temporal/Datalog NoSQL DB (low-activity, study material).
- [[db|diesel]] — established Rust ORM + query builder (sub-section landing for the historical multi-topic file).

### SQL drivers / clients

- [[mysql]] — pure-Rust MySQL/MariaDB driver (`mysql` + `mysql_async`).
- [[rusqlite]] — canonical SQLite-from-Rust crate.
- [[tiberius]] — async Microsoft SQL Server (TDS) client.
- [[adbc]] — Apache Arrow Database Connectivity, the Arrow-native JDBC/ODBC successor.

### Connection pools

- [[deadpool]] — most popular async pool today; multi-runtime.
- [[bb8]] — Tokio-only async pool, smallest API surface.
- [[mobc]] — generic async pool inspired by Go's `database/sql`.
- [[r2d2]] — synchronous predecessor; still the right pick for blocking drivers.

### Schema, migrations, query building

- [[seaquery]] — dynamic SQL query builder (foundation of SeaORM).
- [[barrel]] — Rust schema-builder DSL.
- [[refinery]] — embedded SQL migration runner (Flyway-style).
- [[sqlparser]] — canonical Rust SQL parser (used by DataFusion, Ballista, GlueSQL).

### Caching and concurrent maps

- [[cache]] — Moka (the modern default) and `quick_cache`.
- [[map_cache_storage]] — DashMap, the concurrent-hashmap workhorse.
- [[memory_storage]] — evmap, lock-free eventually-consistent multi-map.
- [[compilation_cache]] — sccache (very different scope: build cache).

### Specialised formats

- [[lance_data_format]] — Lance, the modern columnar format for ML.

### ML × DB integration

- [[ml_letsql]] — LetSQL / xorq SAM-UDF demo on candle + DataFusion.

### Crypto

- [[crypto]] — RustCrypto org overview, with POLYVAL focus.

### DataFusion family (cross-link)

- [[datafusion/README|datafusion/]] — DataFusion proper, Comet, Blaze, Iceberg, Vortex, Delta, LakeSoul, Gluten, RAPIDS.

## Keywords

`#rust` `#data` `#database` `#driver` `#pool` `#sql` `#cache` `#format`

## See also

- [[../../../db/README|db/]] — database substrates (Postgres, MySQL, SurrealDB the server, GreptimeDB the server, vector DBs, time-series, formats, fabrics).
- [[../io/README|programming/rust/io]] — file/object IO, parsers, JSON, XLS, HDFS, object_store.
- [[../../../ml/data/README|ml/data]] — ML-specific dataset / vector / embedding tooling.
- [[../tooling/README|programming/rust/tooling]] — Rust-side dev tools that overlap with the build cache here.
