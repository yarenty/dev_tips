---
title: Rust SQL engines, parsers, and UDF substrates
keywords: [sql-engine, udf, sql-parser, datafusion, rust]
status: reviewed
review_date: 2026/05/03
---

# Rust SQL engines, parsers, and UDF substrates

This section is about **building or extending SQL engines** with Rust — parsers, planners, UDFs across multiple engines, and the SQL-engine projects most relevant to the Rust world. It is **not** about Rust SQL clients, ORMs, or query builders — those live in [[../data/README|programming/rust/data/]] (sqlx, diesel, SeaQuery, sqlparser-rs, …).

The dominant theme of the section is **UDFs** (user-defined functions) — Rust's safety story makes it an excellent language for compiling extensions that get loaded into another database's address space.

## Decision shortcut

| You need… | Reach for | Notes |
|---|---|---|
| Write SQLite extensions in Rust | [[sqlite_loadable]] | The canonical framework (Alex Garcia). |
| Write MariaDB/MySQL UDFs in Rust | [[udf_lib]] | Most polished cross-engine Rust UDF story. |
| Write SQLite UDFs **safely** for untrusted users | [[wasm_for_libsql]] | libSQL's WASM-sandboxed alternative. |
| Excel UDFs in Rust (Windows-only) | [[excel_udf_in_rust]] | XLL via `xladd`/`xladd-derive`. |
| Spark UDF in Rust | [[spark]] (and don't) | Use Blaze/Gluten/Comet to swap the executor instead. |
| Hive UDF in Rust | [[hive]] | Same JVM-bridge cost; legacy system. |
| Use DataFusion's SQL planner directly | [[datafusion]] | Reusable independent of physical execution. |
| Smaller MariaDB-focused SQL parser | [[sql_parse]] | Span-precise diagnostics; for the ANSI-broad option use [[../data/sqlparser|sqlparser-rs]]. |
| Instant REST + GraphQL + SQL on top of files | [[roapi]] | DataFusion-backed; the "PostgREST for Parquet". |
| Self-host a Snowflake-shaped warehouse in Rust | [[databend]] | Or look at GreptimeDB / StarRocks. |
| Run SQLite as an engine (design, WAL, vtabs) | [[sqlite]] | For the *client* see [[../data/rusqlite|rusqlite]]. |
| Talk to Snowflake from Rust | [[snowflake]] | Use ADBC's Snowflake driver ([[../data/adbc|ADBC]]). |
| Talk to SQL Server from Rust | [[rust_libs]] | Pointer to [[../data/tiberius|Tiberius]]. |
| Document / diagram query plan transformations | [[qpml]] | Andy Grove's YAML DSL. |
| Reading list to learn SQL-engine internals | [[books]] | Andy Grove + CMU 15-445 + Petrov. |

## Articles by theme

### SQL engine projects (databases)

- [[cratedb]] — distributed Postgres-wire SQL on Lucene/ES (Java; filed here historically).
- [[databend]] — Rust-built Snowflake-shaped data warehouse.
- [[mariadb]] — MySQL fork; included for the Rust UDF story.
- [[snowflake]] — managed cloud warehouse, plus the Rust client paths (ADBC).
- [[sqlite]] — engine design + Rust extension paths.

### UDF design across engines

- [[udf_lib]] — `udf` crate (MariaDB/MySQL), the most polished Rust UDF framework.
- [[excel_udf_in_rust]] — XLL via `xladd` (Windows).
- [[hive]] — Hive UDF + the JVM-bridge angle.
- [[spark]] — Spark UDF + the modern alternatives (Blaze / Gluten / Comet).
- [[sqlite_loadable]] — `sqlite-loadable-rs` (Alex Garcia).
- [[wasm_for_libsql]] — libSQL's WASM-sandboxed UDFs.

### Parser / planner

- [[datafusion]] — DataFusion's reusable SQL planner.
- [[sql_parse]] — small MariaDB-focused parser; complement to [[../data/sqlparser|sqlparser-rs]].

### Adapters / wrappers

- [[roapi]] — read-only REST/GraphQL/SQL API over any data source (DataFusion-backed).
- [[rust_libs]] — SQL Server / T-SQL UDF angle, points at [[../data/tiberius|Tiberius]].

### Niche / specialised

- [[qpml]] — Query Plan Markup Language (Andy Grove's YAML diagram DSL).
- [[rawsql]] — load named SQL snippets from files (legacy 2018 crate; `sqlx::query_file!` is the modern answer).

### Reading list

- [[books]] — *How Query Engines Work* + CMU DB courses + Petrov + Kleppmann.

## See also (cross-section)

- [[../data/README|programming/rust/data/]] — Rust SQL clients, ORMs, query builders, drivers.
- [[../data/datafusion/README|programming/rust/data/datafusion/]] — the DataFusion engine + ecosystem (Blaze, Gluten, Comet, Iceberg, Delta).
- [[../../../db/README|db/]] — the broader database section (substrate, distributed, time-series, NoSQL).
- [[../../../db/udf/README|db/udf/]] — cross-engine UDF semantics.
- [[../web/wasmtime|programming/rust/web/wasmtime]] — the WASM runtime that powers libSQL UDFs.
- [[../interop/to_java]] — `jni-rs`; the JVM-bridge mechanics behind Hive/Spark Rust UDFs.

## Keywords

`#sql-engine` `#udf` `#sql-parser` `#datafusion` `#rust` `#engine-extension`
