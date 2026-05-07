---
title: deadpool — async object pool
main_link: https://crates.io/crates/deadpool
keywords: [deadpool, connection-pool, async, tokio, async-std, database, redis]
status: reviewed
---

# deadpool — async object pool

**Main link:** <https://crates.io/crates/deadpool>

## Summary

`deadpool` is "a dead simple async pool for connections and objects of any type" — and in 2024 it's the most popular async pool in the Rust ecosystem, with first-party adapter crates for nearly every common backend (`deadpool-postgres`, `deadpool-redis`, `deadpool-lapin`, `deadpool-sqlite`, `deadpool-r2d2`, `deadpool-diesel`). The core crate offers two pools: a **managed** pool that creates/recycles objects on demand (`Manager` trait), and an **unmanaged** pool for objects you build yourself.

## Insight

Default to `deadpool` when starting a new async-Rust project that needs DB/Redis/AMQP pooling and doesn't already use `sqlx` (which has its own built-in pool). Why deadpool over `bb8` or `mobc`: (a) the largest official adapter set, (b) supports both Tokio and async-std runtimes via feature flags, (c) richer hooks (post-create/pre-recycle callbacks, timeouts split per-acquire/per-create/per-recycle, status struct exposing live counts). Gotchas: the per-backend adapter crates version independently from `deadpool` core — pin them carefully; `deadpool-postgres` wraps `tokio-postgres` (not `sqlx`), so if you're on `sqlx` you don't need it. The "managed" vs "unmanaged" distinction often confuses newcomers — managed is what 99% of users want.

## Similar / related topics

- [[bb8]] — Tokio-only async pool, smaller API surface, the historical alternative.
- [[mobc]] — Go-`database/sql`-inspired async pool; less popular today.
- [[r2d2]] — synchronous predecessor; still the right pick for blocking drivers like classic `diesel`.
- `sqlx::Pool` — built into `sqlx`; don't add deadpool on top.
- `tokio-postgres` — most common deadpool backend.

## Internal links
<!-- reviewed -->
- [[bb8]]
- [[mobc]]
- [[r2d2]]
- [[../concurrency/tokio|tokio]]
- [[../net/README|Rust net]] — `deadpool-redis`, `deadpool-lapin` etc. live there in spirit

## Keywords

`#deadpool` `#connection-pool` `#async` `#tokio` `#postgres` `#redis`

## References / raw notes

- Crate: <https://crates.io/crates/deadpool>
- Repo: <https://github.com/bikeshedder/deadpool>

Two pool flavours:

- **Managed** (`deadpool::managed::Pool`) — implement the `Manager` trait (`create`, `recycle`); pool calls them as needed. Enabled via the `managed` feature. Use this for connection pools.
- **Unmanaged** (`deadpool::unmanaged::Pool`) — you push pre-built objects in; pool just hands them out. Enabled via `unmanaged`.

First-party adapters: `deadpool-postgres`, `deadpool-redis`, `deadpool-sqlite`, `deadpool-lapin`, `deadpool-diesel`, `deadpool-r2d2`, `deadpool-amqprs`, `deadpool-redis-cluster`, `deadpool-runtime`, …
