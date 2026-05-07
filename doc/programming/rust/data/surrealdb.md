---
title: SurrealDB — Rust client SDK
main_link: https://docs.rs/surrealdb/
keywords: [surrealdb, multi-model, client, sdk, embedded]
status: reviewed
---

# SurrealDB — Rust client SDK

**Main link:** <https://docs.rs/surrealdb/>

## Summary

This page covers the **Rust SDK** for SurrealDB (`surrealdb` crate), not the database itself — for the database, its license history, and operational story see [[db/relational/surrealdb|SurrealDB (substrate)]] reviewed in P5.S. The crate exposes a single `Surreal<C>` client generic over the connection type: in-memory (`Surreal::new::<Mem>`), embedded RocksDB/SurrealKV (`File`/`SurrealKV`), or remote over WebSocket/HTTP (`Ws`, `Wss`, `Http`, `Https`). The SDK is async (Tokio-only), and queries are written in SurrealQL, executed via `db.query("...")` or via the typed builder.

## Insight

The Rust SDK is unusually **flexible for an "official client"** — the same crate API works against (a) an embedded in-process SurrealDB (no server), (b) a SurrealDB server over the wire, or (c) the SurrealKV storage engine. That makes it natural to build apps that scale from `cargo run` (embedded) up to a remote cluster without code changes — a real differentiator vs Postgres/Mongo SDKs. Gotchas: the embedded variant pulls in a *lot* of dependencies (RocksDB / SurrealKV, full query engine) — your binary balloons; the API has churned across 1.x → 2.x — pin versions and re-check examples; the auth model (root / namespace / database / scope users) doesn't map cleanly to "Postgres roles" — read the security docs.

## Similar / related topics

- [[db/relational/surrealdb|SurrealDB (server-side article)]] — license, multi-model story, operational concerns.
- `mongodb` (Rust driver) — closest analogue for "official multi-model client".
- `tokio-postgres` / `sqlx` — relational alternatives.
- `gluesql` — much smaller embedded SQL engine.

## Internal links
<!-- reviewed -->
- [[db/relational/surrealdb|SurrealDB (substrate)]]
- [[gluesql]]
- [[wooridb]]
- [[../concurrency/tokio|tokio]]

## Keywords

`#surrealdb` `#multi-model` `#client` `#sdk` `#embedded`

## References / raw notes

- Crate: <https://crates.io/crates/surrealdb>
- Docs: <https://docs.rs/surrealdb/>
- Project site: <https://surrealdb.com/>
- Substrate-side article: [[db/relational/surrealdb|db/relational/surrealdb]]

The original stub here was just a back-link to `db/surrealDB.md`; this article was rewritten to focus on the client-SDK angle.
