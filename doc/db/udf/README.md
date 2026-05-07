---
title: User-defined functions (UDFs)
keywords: [udf, user-defined-functions, ffi, wasm, sandbox, datafusion, postgres, spark]
status: reviewed
review_date: 2026/05/03
---

# User-defined functions (UDFs)

A *UDF* is any function you can call from SQL that the engine itself didn't
ship. The two big axes are:

1. **Where it runs** — *in-engine* (linked into the database process; fast,
   trusted, language-restricted) vs *external* (separate process / sandbox /
   shared library; slower, safer, polyglot).
2. **What it returns** — scalar (1 row in → 1 row out), aggregate
   (n rows in → 1 row out), table-valued (n rows in → m rows out), window.

## In-engine vs external

| Trait | In-engine | External |
|---|---|---|
| Performance | Best (no IPC) | Lower (IPC / FFI / WASM call overhead) |
| Safety | Crashes the engine if buggy | Sandboxed |
| Language | Engine's native (C/C++/Java/Rust) or a "trusted" embedded language | Anything that speaks the boundary protocol |
| Examples | Postgres `LANGUAGE c`, MySQL UDF `.so`, DataFusion native `ScalarUDF` | Postgres `plpython3u`, SingleStore WASM UDFs, DataFusion external UDFs over FFI |

## Articles in this section

- [[external_udfs]] — external UDFs via dynamic libraries and Arrow FFI;
  the design pattern for polyglot, zero-copy UDFs against engines like
  DataFusion.

## External pointers

- **Postgres procedural languages** — `plpgsql`, `plpython3u`, `plperlu`,
  `plv8`. The `u` flavours are *un*trusted (full host access).
- **SingleStore WASM UDFs** — sandboxed UDFs in any WASM-compiled language.
  <https://docs.singlestore.com/cloud/reference/code-engine-powered-by-wasm/>
- **DataFusion `ScalarUDF` / `AggregateUDF`** — Rust-native UDFs.
  <https://datafusion.apache.org/library-user-guide/adding-udfs.html>
- **Spark UDFs** — Python (Arrow-batched), Scala, Java; pandas UDFs for
  vectorised execution.
- **DuckDB UDFs** — Python and C++; Arrow-shaped boundaries.

## See also

- [[db/relational/README|relational]] — most UDFs live in relational engines.
- [[db/analytics/README|analytics]] — vectorised analytics engines (DataFusion,
  DuckDB, Spark) are where modern UDF design lives.
- [[db/formats/README|formats]] — Arrow is the de-facto wire format for
  zero-copy UDF boundaries.

## Keywords

`#udf` `#user-defined-functions` `#ffi` `#wasm` `#sandbox` `#datafusion` `#postgres` `#spark` `#duckdb`
