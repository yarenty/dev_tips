---
title: mobc — generic async connection pool
main_link: https://crates.io/crates/mobc
keywords: [mobc, connection-pool, async, tokio, async-std, database]
status: reviewed
review_date: 2026/05/03
---

# mobc — generic async connection pool

**Main link:** <https://crates.io/crates/mobc>

## Summary

`mobc` is a generic async connection pool inspired by `r2d2` and Go's `database/sql` package. It supports both Tokio and async-std runtimes via feature flags and offers the standard `ConnectionManager` trait for plugging in any backend (`mobc-postgres`, `mobc-redis`, `mobc-reql`, `mobc-arangors`, `mobc-lapin`, …).

## Insight

`mobc` exists in roughly the same niche as `bb8` and `deadpool` and works fine, but it has noticeably less mindshare than either today — fewer maintained adapters, slower release cadence, fewer downstream production users. Reach for it if (a) you specifically want the Go-`sql.DB`-style API (max idle / max open / lifetime knobs that match Go semantics one-for-one), or (b) you need an adapter that only ships as `mobc-<backend>`. Otherwise the modern default is `deadpool`. Gotchas: the runtime feature flags (`tokio`, `async-std`) are not additive — pick one; metrics support requires the `prometheus` feature.

## Similar / related topics

- [[deadpool]] — the modern default async pool.
- [[bb8]] — Tokio-only, smaller API; the other historical choice.
- [[r2d2]] — synchronous predecessor.
- Go `database/sql` — direct API inspiration.

## Internal links
- [[deadpool]]
- [[bb8]]
- [[r2d2]]
- [[../concurrency/tokio|tokio]]

## Keywords

`#mobc` `#connection-pool` `#async` `#tokio` `#database`

## References / raw notes

- Crate: <https://crates.io/crates/mobc>
- Repo: <https://github.com/importcjj/mobc>

> A generic connection pool with async/await support. Inspired by r2d2 and Go's `database/sql` package.
