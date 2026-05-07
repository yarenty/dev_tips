---
title: rusqlite — SQLite bindings for Rust
main_link: https://crates.io/crates/rusqlite
keywords: [rusqlite, sqlite, embedded, database, libsql, turso]
status: reviewed
---

# rusqlite — SQLite bindings for Rust

**Main link:** <https://crates.io/crates/rusqlite>

## Summary

`rusqlite` is the canonical Rust binding for SQLite. It wraps the C library (vendored by default via the `bundled` feature, no system SQLite required) with an ergonomic safe API: prepared statements, parameter binding by name or index, row mappers, transactions, blob-IO, virtual tables, custom functions, FTS5, R-Tree. It is **synchronous** by design — SQLite itself is process-local, so async wrappers don't help latency, only ergonomic composition with Tokio.

## Insight

For "I want a real database in my Rust program with zero ops", `rusqlite` is the answer 90% of the time. Combine with `r2d2-sqlite` for connection pooling (SQLite pools matter even though SQLite is in-process — to avoid lock contention with many threads writing). The popular alternatives are: **`libsql`** (Turso's open fork, adds replication, embedded HTTP, vector indexing — wire-compatible with SQLite), **`sqlite3-rs`** (a pure-Rust re-implementation, niche), and **`sqlx` with the SQLite backend** (compile-time-checked queries, but uses libsqlite3 underneath too). Gotchas: enable `bundled` to avoid version-skew bugs against system SQLite, especially on macOS where the system version lags; remember SQLite's "one writer at a time" rule (use WAL mode); the `chrono`/`time`/`uuid`/`serde_json` features are opt-in and gate the corresponding type adapters.

## Similar / related topics

- `libsql` — Turso's open SQLite fork with replication, vector index, server mode.
- `sqlx` (sqlite backend) — compile-time-checked queries on top of libsqlite3.
- `sqlite3-rs` — pure-Rust SQLite reimplementation (experimental).
- `diesel` (sqlite backend) — sync ORM.
- `r2d2-sqlite` — pooling adapter.

## Internal links
- [[r2d2]]
- [[programming/rust/sql_engine/diesel|diesel]]
- [[barrel]] — schema migrations
- [[refinery]] — raw-SQL migrations

## Keywords

`#rusqlite` `#sqlite` `#embedded` `#database` `#libsql`

## References / raw notes

- Crate: <https://crates.io/crates/rusqlite>
- Repo: <https://github.com/rusqlite/rusqlite>

> Rusqlite is an ergonomic wrapper for using SQLite from Rust.
