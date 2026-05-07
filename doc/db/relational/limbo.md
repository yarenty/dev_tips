---
title: "Limbo — SQLite-compatible Rust rewrite from Turso"
main_link: https://github.com/tursodatabase/limbo
keywords: [limbo, turso, sqlite, rust, embedded, io-uring, oltp]
status: reviewed
review_date: 2026/05/03
---

# Limbo — SQLite-compatible Rust rewrite from Turso

**Main link:** <https://github.com/tursodatabase/limbo>

Turso (the company): <https://turso.tech/> · Original SQLite: <https://www.sqlite.org/>

## Summary

Limbo is an in-process OLTP database engine written in Rust by the Turso team, designed as a from-scratch reimplementation of SQLite. It targets full SQLite compatibility for SQL dialect, on-disk file format, and the C API, while adding modern niceties: language bindings for JavaScript/Wasm, Rust, Go, Python, and Java; asynchronous I/O on Linux via `io_uring`; and a roadmap that includes `BEGIN CONCURRENT` for higher write throughput, vector indexing, and stricter schema management.

Note: in 2024–2025 the project is sometimes referred to under the broader "Turso DB" umbrella as Turso has gradually absorbed/renamed it; the GitHub repo path is the canonical source of truth.

## Insight

Why this exists at all: SQLite is one of the most-deployed pieces of software on Earth, and it is excellent — but it is a single-threaded C codebase with limitations that are hard to lift without changing the core (no async I/O, single writer per file, conservative defaults, test suite is famously proprietary). A clean-room Rust rewrite gets you:

- **Memory safety** without the C maintenance burden.
- **Async I/O** that fits the modern server / serverless story (each tenant DB is a file; you want to scale across many of them on one node).
- **Wasm bindings** that work in browsers and edge runtimes (Cloudflare Workers, Vercel Edge, Deno Deploy).
- **Room to add features SQLite never will**, like vector search and BEGIN CONCURRENT.

Honest reality check (as of late 2025):

- **Don't use Limbo as a drop-in SQLite replacement in production yet.** Compatibility is a moving target; the test surface is huge. Watch the issue tracker.
- **For an embedded DB right now, plain SQLite is still the answer.** Use Limbo when you specifically want one of the above features, or you're betting on Turso's edge-DB platform.
- **Compare with libSQL** — Turso's other fork, which is closer to SQLite's C codebase with selected patches. Limbo is the more aggressive long-term bet; libSQL is the production-ready stepping-stone.

## References / raw notes

Headline features (from the README):

- SQLite compatibility for SQL dialect, file format, and C API.
- Language bindings: JavaScript/WebAssembly, Rust, Go, Python, Java.
- Asynchronous I/O on Linux via `io_uring`.
- OS support: Linux, macOS, Windows.

On the roadmap:

- `BEGIN CONCURRENT` for improved write throughput.
- Indexing for vector search.
- Improved schema management (better `ALTER` support, strict column types by default).

## Similar / related topics

- [SQLite](https://www.sqlite.org/) — the original; still the right default for embedded.
- [libSQL](https://github.com/tursodatabase/libsql) — Turso's more conservative SQLite fork.
- [[xlite]] — SQLite virtual-table extension that lets you query spreadsheets.
- [[postgresql]] — what to use the moment you outgrow single-file embedded.
- [DuckDB](https://duckdb.org/) — the analytical equivalent of "embedded SQLite".

## Internal links

- [[postgresql]]
- [[xlite]]
- [[toydb]]

## Keywords

`#limbo` `#turso` `#sqlite` `#rust` `#embedded` `#io-uring` `#oltp` `#db`
