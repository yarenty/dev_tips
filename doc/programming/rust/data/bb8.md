---
title: bb8 — async connection pool
main_link: https://crates.io/crates/bb8
keywords: [bb8, connection-pool, async, tokio, database, rust]
status: reviewed
review_date: 2026/05/03
---

# bb8 — async connection pool

**Main link:** <https://crates.io/crates/bb8>

## Summary

`bb8` is a full-featured async connection pool for Tokio, originally a port of `r2d2`'s API to async-await. It is backend-agnostic: any type implementing the `ManageConnection` trait can be pooled (Postgres, Redis, Bolt/Neo4j, Tiberius/MSSQL, Diesel, lapin/AMQP, …). Compared to `deadpool` and `mobc`, `bb8` has the smallest API surface and stays closest to the historical `r2d2` shape, which makes it the easiest pool to learn if you already know `r2d2`.

## Insight

Pick `bb8` when you want a thin, opinionated, Tokio-only pool with the same mental model as `r2d2` — connections are acquired with `pool.get().await` and returned via `Drop`. The trait you implement is small (`connect`, `is_valid`, `has_broken`) which makes wrapping a new driver straightforward. Gotchas: bb8 is **Tokio-only** (no `async-std`/`smol` runtime support), the `bb8-postgres`/`bb8-redis` adapters lag the upstream crate by a release or two, and there is no built-in metrics/event hooks (compare `deadpool`'s richer state). For most new code in 2024+, `deadpool` has more momentum and a wider adapter ecosystem; `bb8` survives because it's tiny and stable.

## Similar / related topics

- [[deadpool]] — the most-popular async pool today, multi-runtime, richer hook surface.
- [[mobc]] — async pool inspired by Go's `database/sql` pool; less popular than the above two.
- [[r2d2]] — the synchronous predecessor; same mental model, blocking API.
- `sqlx` — has its own built-in pool, no `bb8`/`deadpool` needed.
- `tokio-postgres` — the canonical bb8 backend.

## Internal links
- [[deadpool]]
- [[mobc]]
- [[r2d2]]
- [[tiberius]] — uses `bb8-tiberius` for MSSQL pooling
- [[../concurrency/tokio|tokio]]

## Keywords

`#bb8` `#connection-pool` `#async` `#tokio` `#database`

## References / raw notes

- Crate: <https://crates.io/crates/bb8>
- Repo: <https://github.com/djc/bb8>

Adapter crates (most live in-tree under the `bb8` repo):

| Backend | Adapter |
|---|---|
| `tokio-postgres` | `bb8-postgres` |
| `redis` | `bb8-redis` |
| `rsmq` | `rsmq_async` |
| `bolt-client` | `bb8-bolt` |
| `diesel` | `bb8-diesel` |
| `tiberius` | `bb8-tiberius` |
| `nebula-client` | `bb8-nebula` |
| `memcache-async` | `bb8-memcached` |
| `lapin` | `bb8-lapin` |
