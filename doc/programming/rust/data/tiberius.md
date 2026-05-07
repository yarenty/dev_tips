---
title: tiberius — Microsoft SQL Server async client
main_link: https://crates.io/crates/tiberius
keywords: [tiberius, mssql, sqlserver, tds, async, driver]
status: reviewed
review_date: 2026/05/03
---

# tiberius — Microsoft SQL Server async client

**Main link:** <https://crates.io/crates/tiberius>

## Summary

`tiberius` is a native Microsoft SQL Server (TDS) client for Rust, originally extracted from `prisma-engines`. It implements the TDS wire protocol asynchronously (works over any `AsyncRead + AsyncWrite` — Tokio, async-std, smol), with TLS via `native-tls` or `rustls`, BCP, named-instance discovery via SQL Browser, and Windows-Authentication on Windows hosts. It is intentionally protocol-only: no connection pool, no query builder, no ORM — pair with `bb8-tiberius`/`deadpool-tiberius`/`mobc-tiberius` for pooling.

## Insight

`tiberius` is essentially **the** Rust story for talking to MSSQL. The alternatives are wrapping ODBC (via `odbc-api` or `r2d2-odbc`) which is platform-painful (driver install, version skew, Unicode issues), or going through a sidecar. Tiberius's headline feature is being **runtime-agnostic**: most async DB drivers are tied to Tokio, but tiberius accepts any AsyncRead/Write socket, so it works in Lambda, edge, or async-std code without contortions. Gotchas: Kerberos / Windows Authentication is only fully supported on Windows hosts (Linux needs `kinit` + GSSAPI feature flag); Azure SQL / managed-identity auth requires the optional `aad` feature; the lack of a built-in pool surprises newcomers — you really do need `bb8-tiberius` (the most common adapter). Compared to .NET's `SqlClient`, tiberius is solid for OLTP but lighter on bulk-copy and Service-Broker support.

## Similar / related topics

- `odbc-api` — generic ODBC client; the pre-tiberius Rust→MSSQL escape hatch.
- [[bb8]] + `bb8-tiberius` — recommended pool combo.
- `prisma-engines` — original home of tiberius.
- .NET `Microsoft.Data.SqlClient` — the reference implementation tiberius mirrors.

## Internal links
- [[bb8]]
- [[deadpool]]
- [[mobc]]
- [[../concurrency/tokio|tokio]]

## Keywords

`#tiberius` `#mssql` `#sqlserver` `#tds` `#async`

## References / raw notes

- Crate: <https://crates.io/crates/tiberius>
- Repo: <https://github.com/prisma/tiberius>

A native Microsoft SQL Server (TDS) client for Rust.

**Goals**

- A correct implementation of the TDS protocol.
- Asynchronous network IO.
- Independent of the network protocol (works on Tokio / async-std / smol).
- Support for current Linux, Windows, and macOS releases.

**Non-goals**

- Connection pooling — use `bb8`, `mobc`, `deadpool` (or any other async pool).
- Query building.
- Object-relational mapping.
