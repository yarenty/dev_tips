---
title: SQLite — engine design and Rust extensions
main_link: https://www.sqlite.org/
keywords: [sqlite, engine, virtual-tables, wal, loadable-extensions, rust]
status: reviewed
review_date: 2026/05/03
---

# SQLite — engine design and Rust extensions

**Main link:** <https://www.sqlite.org/>

## Summary

SQLite-as-engine: this article covers the **engine itself** (B-tree storage, WAL, virtual tables, loadable extensions, the application-defined-function vs loadable-extension distinction) and the Rust pathways for extending it. For the Rust *client* side (open a `.db`, run queries from a Rust app), see [[../data/rusqlite|rusqlite]]. For the canonical Rust framework for *writing* loadable extensions, see [[sqlite_loadable]].

## Insight

SQLite's design philosophy is what makes it interesting as an engine: it is **in-process** (the engine runs inside your application's address space — no daemon, no IPC), single-file, public domain, and explicitly conservative about feature scope. The two SQLite extension mechanisms are easy to confuse:

- **Application-defined functions (UDFs)** — registered via the C API (`sqlite3_create_function_v2`) on a single open connection, scoped to that connection, and have to be defined in your application code. The Rust path is `rusqlite::Connection::create_scalar_function` / `create_aggregate_function`. Use these when the function is part of your app and you don't want to ship a separate `.so`.
- **Loadable extensions** — compiled dynamically-linked libraries (`.so` / `.dylib` / `.dll`) loaded with `SELECT load_extension('path')` or `sqlite3_load_extension`, with a discoverable entry-point function name like `sqlite3_<filename>_init`. They can be loaded by *any* SQLite consumer (the `sqlite3` CLI, Python's `sqlite3`, Node, …) — that's the reuse story. The Rust path here is [[sqlite_loadable]] (Alex Garcia's `sqlite-loadable-rs`).

Other engine-shaped concepts worth knowing for any SQLite work: **virtual tables** (the mechanism behind FTS5, R-Tree, the CSV reader, sqlite-vec, sqlite-vss); **WAL mode** (concurrent readers + one writer; the default for any production use); **STRICT tables** (opt-in column type enforcement, since 3.37); **`sqlite3_analyzer`** for understanding storage layout.

The Rust *rewrites* of SQLite to keep an eye on: **libSQL** (Turso) is a fork that adds replication and WASM UDFs (see [[wasm_for_libsql]]); **Limbo** is Turso's pure-Rust SQLite-compatible rewrite from scratch (alpha). Both are still narrower than upstream SQLite for production use today; rusqlite + the C SQLite library remain the safe default.

## Similar / related topics

- [[../data/rusqlite|rusqlite]] — canonical Rust SQLite client (and the home of application-defined functions).
- [[sqlite_loadable]] — Rust framework for writing loadable SQLite extensions.
- [[wasm_for_libsql]] — Turso/libSQL's WASM-sandboxed UDFs.
- **sqlite-vec / sqlite-vss / sqlite-utils / sqlean / sqlite-zstd** — the loadable-extension ecosystem to draw inspiration from.
- [[../../../db/relational/limbo|Limbo]] — Turso's pure-Rust SQLite rewrite.

## Internal links

- [[README]]
- [[../data/rusqlite|rusqlite — Rust client]]
- [[sqlite_loadable]] — extensions in Rust.
- [[wasm_for_libsql]] — WASM UDFs (libSQL).
- [[../../../db/relational/limbo|Limbo (pure-Rust SQLite)]]
- [[../../../db/relational/postgresql|PostgreSQL]] — the obvious "client/server" alternative.

## Keywords

`#sqlite` `#engine` `#virtual-tables` `#wal` `#loadable-extensions` `#rust-extension`

## References / raw notes

- SQLite home: <https://www.sqlite.org/>
- Loadable extensions C API: <https://www.sqlite.org/loadext.html>
- Application-defined functions C API: <https://www.sqlite.org/c3ref/create_function.html>
- Virtual tables: <https://www.sqlite.org/vtab.html>
- WAL mode: <https://www.sqlite.org/wal.html>
- Phiresky's `sqlite-zstd` extension (Rust example): <https://github.com/phiresky/sqlite-zstd>
- Original tutorial article: <https://github.com/polyrand/rust-sqlite-ext-example>

### Loadable-extension entry point in Rust (low-level)

If you don't use [[sqlite_loadable]], the bare-bones entry-point pattern looks like this:

```rust
#[no_mangle]
pub unsafe extern "C" fn sqlite3_regex_init(
    db: *mut ffi::sqlite3,
    _pz_err_msg: &mut &mut std::os::raw::c_char,
    p_api: *mut ffi::sqlite3_api_routines,
) -> c_int {
    loadable_extension_init(p_api);

    match init(db) {
        Ok(()) => {
            log::info!("[regex-extension] init ok");
            ffi::SQLITE_OK
        }
        Err(e) => {
            log::error!("[regex-extension] init error: {e:?}");
            ffi::SQLITE_ERROR
        }
    }
}
```

Notes:

- `#[no_mangle]` is required so the linker exposes the symbol with the exact name SQLite is looking for.
- The function is `unsafe` — you receive raw pointers from C.
- Return `SQLITE_OK_LOAD_PERMANENTLY` instead of `SQLITE_OK` if you want the extension to persist across connection closes.
- Function name follows `sqlite3_<filename>_init` convention, e.g. `sqlite3_regex_ext_init` for an extension at `regex_ext.so`.

```sql
SELECT load_extension('path/to/loadable/extension/regex_ext.so', 'sqlite3_regex_init');
```

For anything beyond a toy, **don't write the unsafe entry-point and FFI by hand** — use [[sqlite_loadable]].
