---
title: sqlite-loadable-rs — write SQLite extensions in Rust
main_link: https://github.com/asg017/sqlite-loadable-rs
keywords: [sqlite-loadable, sqlite, rust, loadable-extensions, virtual-tables, scalar-functions]
status: reviewed
---

# sqlite-loadable-rs — write SQLite extensions in Rust

**Main link:** <https://github.com/asg017/sqlite-loadable-rs>

## Summary

`sqlite-loadable-rs` is the canonical Rust framework for writing **loadable SQLite extensions** (`.so` / `.dylib` / `.dll` files that any SQLite consumer — the `sqlite3` CLI, Python, Node, Go, your Rust app — can `SELECT load_extension(...)` and immediately use). Created by Alex Garcia (asg017), inspired by `rusqlite`, `pgrx`, and Riyaz Ali's Go equivalent. Supports scalar functions, table functions ("eponymous-only virtual tables"), and full virtual tables (read-only and writeable).

## Insight

This is the path you want for any non-trivial Rust SQLite extension. The mental model: write your function as a normal Rust fn, decorate the entry-point with `#[sqlite_entrypoint]`, register your scalar / table / virtual table inside it, build with `crate-type = ["cdylib"]`, ship the resulting `.so`. The reason this is *the* answer (rather than hand-rolling the FFI as shown in [[sqlite|sqlite.md]]) is that the framework hides the SQLite C ABI's many footguns: result-binding, value-extraction, error-propagation, virtual-table cursor lifecycle, and the `xConnect`/`xCreate`/`xBestIndex`/`xFilter` cycle for vtabs. The same author's downstream extensions (`sqlite-vec`, `sqlite-vss`, `sqlite-xsv`, `sqlite-regex`, `sqlite-base64`, `sqlite-html`, `sqlite-jsonschema`) are the production proof — these are widely used (sqlite-vec especially, in the LLM/RAG world) and all built on this framework. Comparison: **rusqlite has had multiple PRs to add loadable-extension support but none have merged**, so for "I want to compile a `.so` that anyone can `LOAD EXTENSION`", `sqlite-loadable-rs` is the only realistic Rust choice. Versus the **Go equivalent** (`riyaz-ali/sqlite`), Rust wins on binary size and runtime overhead. Versus **the modern alternative — WASM UDFs** in libSQL (see [[wasm_for_libsql]]), `sqlite-loadable` wins on performance and works with vanilla SQLite, but requires you to ship a binary per platform; WASM lets you ship one bytecode that runs anywhere.

## Similar / related topics

- [[sqlite]] — engine background and the alternative bare-FFI path.
- [[../data/rusqlite|rusqlite]] — the Rust SQLite client; uses application-defined functions instead of loadable extensions.
- [[wasm_for_libsql]] — modern alternative for libSQL: WASM-sandboxed UDFs.
- **`pgrx`** — the Postgres-extensions-in-Rust equivalent; the framework that inspired this design.
- **sqlite-vec / sqlite-vss / sqlite-xsv / sqlite-regex** — production extensions built on this framework.

## Internal links

- [[README]]
- [[sqlite]] — SQLite engine background.
- [[../data/rusqlite|rusqlite client]]
- [[wasm_for_libsql]] — WASM UDF alternative.
- [[udf_lib]] — the MariaDB equivalent of "write UDFs in Rust".
- [[../../../db/udf/external_udfs|external UDFs (cross-engine)]]

## Keywords

`#sqlite-loadable` `#sqlite` `#rust` `#loadable-extensions` `#virtual-tables`

## References / raw notes

- Source: <https://github.com/asg017/sqlite-loadable-rs>
- Crate: <https://crates.io/crates/sqlite-loadable>

> A framework for building loadable SQLite extensions in Rust. Inspired by `rusqlite`, `pgx`, and Riyaz Ali's similar SQLite Go library.

### Background

SQLite's runtime loadable extensions allow one to add new scalar functions, table functions, virtual tables, virtual filesystems, and more to a SQLite database connection. These compiled dynamically-linked libraries can be loaded in any SQLite context, including the SQLite CLI, Python, Node.js, Rust, Go, and many other languages.

> **Note** — notice the word *loadable*. Loadable extensions are these compiled dynamically-linked libraries with a suffix of `.dylib` / `.so` / `.dll`. These are different from **application-defined functions** that many language clients support (Python's `.create_function()`, Node's `.function()`, rusqlite's `create_scalar_function`).

Historically the C path (with examples like SpatiaLite, the sqlean project, SQLite's official miscellaneous extensions) was the only realistic option, with C's safety / 3rd-party-dep nightmares included. Riyaz Ali's Go library trades binary size and performance for ergonomics. For Rust, `rusqlite` has had multiple unmerged PRs for loadable-extension support; `sqlite-loadable-rs` is the answer that landed.

### Scalar functions

```rust
// add(a, b)
fn add(context: *mut sqlite3_context, values: &[*mut sqlite3_value]) -> Result<()> {
    let a = api::value_int(values.get(0).expect("1st argument"));
    let b = api::value_int(values.get(1).expect("2nd argument"));
    api::result_int(context, a + b);
    Ok(())
}

// connect(separator, string1, string2, ...)
fn connect(context: *mut sqlite3_context, values: &[*mut sqlite3_value]) -> Result<()> {
    let separator = api::value_text(values.get(0).expect("1st argument"))?;
    let strings: Vec<&str> = values
        .get(1..)
        .expect("more than 1 argument to be given")
        .iter()
        .filter_map(|v| api::value_text(v).ok())
        .collect();
    api::result_text(context, &strings.join(separator))?;
    Ok(())
}

#[sqlite_entrypoint]
pub fn sqlite3_extension_init(db: *mut sqlite3) -> Result<()> {
    define_scalar_function(db, "add", 2, add, FunctionFlags::DETERMINISTIC)?;
    define_scalar_function(db, "connect", -1, connect, FunctionFlags::DETERMINISTIC)?;
    Ok(())
}
```

```sql
sqlite> select add(1, 2);
3
sqlite> select connect('-', 'alex', 'brian', 'craig');
alex-brian-craig
```

### Table functions ("eponymous-only virtual tables")

```rust
define_table_function::<CharactersTable>(db, "characters", None)?;
```

Defining a table function is more involved — see the framework's `characters.rs` example. Once compiled:

```sql
sqlite> .load target/debug/examples/libcharacters
sqlite> select rowid, * from characters('alex garcia');
┌───────┬───────┐
│ rowid │ value │
├───────┼───────┤
│ 0     │ a     │
│ 1     │ l     │
│ 2     │ e     │
│ 3     │ x     │
…
└───────┴───────┘
```

Real-world non-Rust examples of table functions in SQLite: `json_each` / `json_tree`, `generate_series`, the `pragma_*` functions, `html_each`.

### Virtual tables

For tables with a dynamic schema or insert/update support:

```rust
define_virtual_table::<CustomVtab>(db, "custom_vtab", None)?;
// or, for INSERT/UPDATE/DELETE support:
define_virtual_table_writeable::<CustomVtab>(db, "custom_vtab", None)?;
```

```sql
create virtual table xxx using custom_vtab(arg1=...);
select * from xxx;
```

Real-world non-Rust examples of traditional virtual tables: the CSV virtual table, the FTS5 full-text-search extension, the R-Tree extension.

### Building the examples

```sh
cargo build --example hello
sqlite3 :memory: '.load target/debug/examples/hello' 'select hello("world");'
# hello, world!
```

```sh
# Build all the examples in release mode (output at target/release/examples/*.dylib)
cargo build --examples --release
```

### Real-world projects built on this framework

- **sqlite-vec** — vector search in SQLite (modern alternative to `sqlite-vss`).
- **sqlite-vss** — older Faiss-based vector search.
- **sqlite-xsv** — extremely fast CSV/TSV parser as a SQLite virtual table.
- **sqlite-regex** — fast and safe regex in SQLite.
- **sqlite-base64** — base64 encoding/decoding.
- **sqlite-html** — HTML scraping in SQL.
- **sqlite-jsonschema** — JSON Schema validation.
