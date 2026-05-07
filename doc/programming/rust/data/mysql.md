---
title: mysql — pure-Rust MySQL driver
main_link: https://crates.io/crates/mysql
keywords: [mysql, mysql_async, driver, database, rust]
status: reviewed
---

# mysql — pure-Rust MySQL driver

**Main link:** <https://crates.io/crates/mysql>

## Summary

The `mysql` crate is a **pure-Rust** synchronous driver for MySQL/MariaDB, with a sibling `mysql_async` crate providing the async/Tokio variant. Both are maintained by `blackbeam` and are the canonical "I need to talk to MySQL from Rust without going through ODBC or sqlx" crates. They support the full MySQL text + binary protocols, prepared statements, named parameters, custom `LOCAL INFILE` handlers, protocol compression, TLS via either `native-tls` or `rustls`, Unix sockets and Windows named pipes, and the `caching_sha2_password` auth plugin used by MySQL 8+.

## Insight

For new code, **`sqlx` is the modern default** for talking to MySQL/MariaDB from async Rust — it gives you compile-time-checked queries, a built-in pool, and database-agnostic code. Reach for the `mysql`/`mysql_async` crates instead when you need (a) a sync API in non-Tokio code (`mysql`), (b) protocol features sqlx doesn't expose (multi-result-sets, custom LOCAL INFILE handlers, MySQL-specific buffer pools), or (c) maximum throughput on a hot loop where sqlx's overhead matters. Pair `mysql`/`mysql_async` with `r2d2`/`bb8`/`deadpool` for pooling — `mysql_async` ships its own `Pool` too. For Diesel users, the official `diesel` MySQL backend uses `mysqlclient-sys` (the C library), not these pure-Rust crates. MariaDB works for the most common queries but has divergence around JSON, sequences, and replication — test on the actual server you'll deploy against.

## Similar / related topics

- `sqlx` — modern default for async MySQL/MariaDB; compile-time query checking.
- `diesel` — sync ORM with MySQL backend (uses C `mysqlclient`).
- `mysql_async` — async sibling of `mysql`.
- `mysqlclient-sys` — FFI wrapper around the official C client.
- [[db/relational/mysql|MySQL (server side)]] — operational/server article.

## Internal links
<!-- reviewed -->
- [[db/relational/mysql|MySQL]]
- [[r2d2]] — `r2d2-mysql`
- [[deadpool]] / [[bb8]] / [[mobc]]
- [[programming/rust/sql_engine/diesel|diesel]]

## Keywords

`#mysql` `#driver` `#database` `#sqlx-alternative` `#tls`

## References / raw notes

- Crate (sync): <https://crates.io/crates/mysql>
- Crate (async): <https://crates.io/crates/mysql_async>
- Repo: <https://github.com/blackbeam/rust-mysql-simple>

Features (sync `mysql` crate):

- macOS, Windows and Linux support
- TLS via `native-tls` or `rustls`
- MySQL text & binary protocols (simple queries + prepared statements)
- Multi-result sets
- Named parameters for prepared statements
- Optional per-connection prepared-statement cache
- Buffer pool
- Packets larger than 2^24
- Unix sockets and Windows named pipes
- Custom `LOCAL INFILE` handlers
- Protocol compression
- Auth plugins: `mysql_native_password` (pre-v8) + `caching_sha2_password` (v8+)
