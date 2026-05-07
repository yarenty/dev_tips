---
title: GlueSQL — embeddable SQL engine with pluggable storage
main_link: https://github.com/gluesql/gluesql
keywords: [gluesql, embedded, sql, sled, wasm, in-memory]
status: reviewed
---

# GlueSQL — embeddable SQL engine with pluggable storage

**Main link:** <https://github.com/gluesql/gluesql>

## Summary

`gluesql` is a SQL database **library** written in Rust: it ships a SQL parser (built on `sqlparser-rs`), a query planner/execution layer, and a pluggable storage trait so you can back the engine with anything — `sled` (default persistent), in-memory, JSON files, CSV, MongoDB, IndexedDB, even a Git tree. The same engine compiles to native and to WebAssembly, which is the headline use case (run SQL inside the browser against IndexedDB or in-memory storage).

## Insight

Reach for GlueSQL when you want **SQL semantics without committing to a specific storage engine**: in-browser apps that want SQL over IndexedDB, mobile apps that want SQL over a JSON blob, services that want a CSV/Parquet-shaped lake queryable via SQL, or testing harnesses that need a no-deps in-memory SQL store. Compared to `rusqlite` it is much smaller in scope (no WAL, no full SQLite compatibility, no real B-trees in most backends), and compared to embedded analytics engines like DuckDB or DataFusion it has weaker query planning. The sweet spot: **"I need SQL surface, my data lives somewhere weird, my workload is small."** Gotchas: the SQL dialect is a strict subset of ANSI (no window functions for a long time, limited DDL); WASM bundle size is non-trivial — check before adding to a frontend.

## Similar / related topics

- [[rusqlite]] — full SQLite via C; pick this when you want a real B-tree DB and aren't in WASM.
- DataFusion — big-data Arrow-native engine; overkill for "SQL over a JSON file".
- DuckDB — OLAP embedded engine; bigger and faster but C++ and not WASM-easy.
- `sled` — the default GlueSQL persistent backend; pure-Rust embedded KV.
- `surrealdb` — multi-model embedded DB; bigger surface area than GlueSQL.

## Internal links
<!-- reviewed -->
- [[sqlparser]] — frontend GlueSQL is built on
- [[surrealdb]]
- [[wooridb]]
- [[datafusion/README|DataFusion]]
- [[rusqlite]]

## Keywords

`#gluesql` `#embedded` `#sql` `#sled` `#wasm`

## References / raw notes

- Repo: <https://github.com/gluesql/gluesql>
- Crate: <https://crates.io/crates/gluesql>

> GlueSQL is a SQL database library written in Rust. It provides a parser (sqlparser-rs), an execution layer, and optional storages (sled or memory) packaged into a single library. Developers can choose to use GlueSQL to build their own SQL database, or as an embedded SQL database using the default storage engine.
