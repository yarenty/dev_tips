---
title: WASM UDFs for libSQL
main_link: https://bindgen.libsql.org/
keywords: [wasm, libsql, turso, udf, sandbox, wasmtime, sqlite]
status: reviewed
review_date: 2026/05/03
---

# WASM UDFs for libSQL

**Main link:** <https://bindgen.libsql.org/>

## Summary

libSQL — Turso's open-source fork of SQLite — supports a UDF model that's distinct from upstream SQLite: **WebAssembly-sandboxed user-defined functions**. You write a Rust function, compile it with `libsql_bindgen` to a WASM module that targets `libsql_wasm_abi`, register the WASM bytecode in the database, and libSQL executes it in a `wasmtime` sandbox each time the function is called from SQL. The browser-based generator at <https://bindgen.libsql.org/> lets you paste Rust source and download a ready-to-load WASM blob.

## Insight

This is the modern alternative to native loadable extensions ([[sqlite_loadable]]) for the libSQL family — and the killer feature is **safety**: a WASM UDF cannot read your filesystem, network, or memory outside its sandbox, so you can run **untrusted** user-supplied UDFs (e.g. multi-tenant SaaS, Turso's edge-database product). It also eliminates the cross-compile pain — one `.wasm` file runs on every libSQL host (x86_64 Linux, arm64 macOS, edge workers, …) instead of one `.so` per platform. Trade-offs vs `sqlite-loadable-rs`: the WASM sandbox costs single-digit-percent throughput per call (wasmtime is fast, but not free), there's no easy path to virtual tables (the WASM ABI currently exposes scalar functions, not vtab cursors), and you're locked into the libSQL fork — the same `.wasm` will not load into upstream SQLite. Reach for it when (a) your workload is multi-tenant Turso/libSQL and you want to expose a UDF surface to users without giving them a path to RCE, or (b) you want one bytecode per UDF instead of N native binaries. Otherwise stay with [[sqlite_loadable]] for performance and reach.

## Similar / related topics

- [[sqlite_loadable]] — the native-binary alternative for SQLite/libSQL extensions.
- **Turso / libSQL** — the SQLite fork providing this feature; modern hosted SQLite-at-the-edge.
- [[../web/wasmtime|wasmtime]] — the WASM runtime libSQL embeds.
- **Postgres `plrust`** — closest analogue in the Postgres world: trusted-language UDFs in Rust.
- [[wasm_for_libsql|libsql_bindgen]] / `libsql_wasm_abi` — the build-side and runtime-side crates.

## Internal links

- [[README]]
- [[sqlite_loadable]] — native-binary alternative.
- [[sqlite]] — engine background.
- [[../web/wasmtime|wasmtime]] — the WASM runtime.
- [[../../../db/relational/limbo|Limbo]] — Turso's pure-Rust SQLite rewrite from scratch.

## Keywords

`#wasm` `#libsql` `#turso` `#udf` `#sandbox` `#wasmtime` `#sqlite`

## References / raw notes

- libSQL home: <https://libsql.org/>
- Bindgen web tool: <https://bindgen.libsql.org/>
- `libsql_bindgen` crate: <https://crates.io/crates/libsql_bindgen>
- `libsql_wasm_abi` crate: <https://crates.io/crates/libsql_wasm_abi>
- `libsql_generate` (the original "compile Rust to libSQL-compatible WASM" repo): <https://github.com/libsql/libsql_generate>
- Turso (managed libSQL): <https://turso.tech/>
