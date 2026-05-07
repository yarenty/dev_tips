---
title: udf — MariaDB/MySQL UDFs in Rust
main_link: https://crates.io/crates/udf
keywords: [udf, mariadb, mysql, rust, basicudf, aggregateudf, gat, cdylib]
status: reviewed
review_date: 2026/05/03
---

# udf — MariaDB/MySQL UDFs in Rust

**Main link:** <https://crates.io/crates/udf>

## Summary

The `udf` crate (by `pluots`, repo: `pluots/sql-udf`) is the canonical Rust framework for writing **MariaDB / MySQL user-defined functions**. You implement the `BasicUdf` trait (and optionally `AggregateUdf` for aggregates) on a struct, decorate the impl block with `#[udf::register]`, build as `crate-type = ["cdylib"]`, drop the resulting `.so` into the server's `plugin_dir`, and register with `CREATE FUNCTION foo … SONAME 'libfoo.so';`. As of 2024, this is **the most polished Rust UDF framework across all SQL engines** — actively maintained, full aggregate support, GAT-based ergonomic trait surface, Docker test harness.

## Insight

Reach for `udf` whenever you find yourself wanting to push CPU-heavy or domain-specific logic (custom regex flavours, statistical functions, vector operations, lookups) into MariaDB/MySQL itself — instead of round-tripping rows out to Rust application code. Compared to the C UDF API the equivalence is roughly 1:1 — `udf` doesn't *expand* what you can do, it just makes it safe (no manual `init`/`deinit` lifecycle, no manual buffer-size tracking, the GATs handle borrow-checker friendliness for the per-row `process` reference). **GATs requirement**: needs Rust ≥ 1.65 (now ancient, but worth flagging). **Lifetime trick**: for string/decimal returns longer than `MYSQL_RESULT_BUFFER_SIZE` (255 bytes) the result string must live in your struct (`process` returns a reference into `&'a self`, not a fresh `String`). **Per-row, not per-batch**: the calling convention is one row at a time — no Arrow, no SIMD pipeline (the engine isn't columnar). For columnar engines, see [[../data/datafusion/README|DataFusion]] (`ScalarUDF` API) or [[sqlite_loadable]] for the per-row SQLite equivalent. **Cross-DB**: the resulting `.so` is **not** loadable into Postgres or SQL Server — for Postgres use `pgrx`; for SQL Server see [[rust_libs|the SQL Server article]]. **Generalization angle**: this article doubles as a reference for "what does a generic Rust UDF library look like?" — see also `pgrx` (Postgres) and `sqlite-loadable-rs` ([[sqlite_loadable]]) for sibling designs.

## Similar / related topics

- **`pgrx`** — the Postgres equivalent: write extensions (UDFs, types, hooks, BGWs) in Rust.
- [[sqlite_loadable]] — the SQLite equivalent: loadable extensions in Rust.
- [[mariadb]] — the engine this crate targets (and its companion article).
- [[../data/mysql|`mysql` / `mysql_async`]] — the Rust client side; complementary, not substitute.
- [[../../../db/udf/external_udfs|external UDFs (cross-engine)]] — the broader survey.

## Internal links

- [[README]]
- [[mariadb]] — the engine and its loading mechanics.
- [[sqlite_loadable]] — the SQLite sibling design.
- [[excel_udf_in_rust]] — same-author Excel sibling (`xladd`).
- [[../../../db/udf/external_udfs|external UDFs (cross-engine survey)]]
- [[../data/mysql|MySQL/MariaDB Rust client]]

## Keywords

`#udf` `#mariadb` `#mysql` `#rust` `#cdylib` `#aggregate-udf`

## References / raw notes

- Crate: <https://crates.io/crates/udf>
- Source: <https://github.com/pluots/sql-udf>
- Docs (`AggregateUdf` trait): <https://docs.rs/udf/latest/udf/trait.AggregateUdf.html>
- Working examples: <https://github.com/pluots/sql-udf/tree/main/udf-examples>

### UDF theory

A basic SQL UDF consists of three exposed C functions: an **init** (check arguments, allocate state), a **process** (return a row's result), and a **deinit** (clean up). Aggregate UDFs add **clear** (per-group reset), **add** (per-row accumulate), and optionally **remove** (window functions).

The `udf` crate trait surface mirrors that — `BasicUdf::{init, process}` plus `AggregateUdf::{clear, add, remove}` — and the proc-macro generates the C-ABI shim symbols MariaDB looks for at `dlsym` time.

### Quickstart

```sh
cargo new --lib my-udf
cd my-udf
cargo add udf
```

```toml
# Cargo.toml
[lib]
crate-type = ["cdylib"]
```

1. Define a struct (data shared between init/process/clear/add/remove).
2. Implement `BasicUdf` on it.
3. (Optional) Implement `AggregateUdf` for aggregates.
4. Decorate each impl block with `#[udf::register]`.
5. `cargo build --release` → `target/release/libmy_udf.so`.
6. Copy into MariaDB/MySQL `plugin_dir` (typically `/usr/lib/mysql/plugin/`).
7. `CREATE FUNCTION my_udf RETURNS … SONAME 'libmy_udf.so';`
8. Use it in SQL.

The function name in SQL is the struct name converted to `snake_case`.

### Struct creation

```rust
/// `sum_int` — adds all arguments as integers, no shared state.
struct SumInt;

/// `avg` — needs a running total across rows in a group.
struct Avg {
    running_total: f64,
}
```

For functions returning buffers (string/decimal) where the output may exceed `MYSQL_RESULT_BUFFER_SIZE` (255 bytes), the result must be held in the struct so `process` can return a reference:

```rust
/// Generate random lipsum that may be longer than 255 bytes.
struct Lipsum {
    res: String,
}
```

### Trait implementation

```rust
use udf::prelude::*;

struct SumInt;

#[register]
impl BasicUdf for SumInt {
    type Returns<'a> = Option<i64>;

    fn init<'a>(
        cfg: &UdfCfg<Init>,
        args: &'a ArgList<'a, Init>,
    ) -> Result<Self, String> {
        // ...
        Ok(Self)
    }

    fn process<'a>(
        &'a mut self,
        cfg: &UdfCfg<Process>,
        args: &ArgList<Process>,
        error: Option<NonZeroU8>,
    ) -> Result<Self::Returns<'a>, ProcessError> {
        // ...
        Ok(Some(0))
    }
}
```

Tip: with rust-analyzer, type `impl BasicUdf for MyStruct {}` and let the IDE auto-fill the function skeletons.

### Compiling

```toml
[lib]
crate-type = ["cdylib"]
```

```sh
cargo build --release
```

> **Version note**: relies on Generic Associated Types (GATs), available on Rust ≥ 1.65 (stable 2022-11-03). Run `rustup update` if you hit compiler errors.

### Symbol inspection

Verify the C-callable functions are present in your `.so` with `nm`:

```sh
# Output of example .so
nm -gC --defined-only target/release/libudf_examples.so
```

```log
00000000000081b0 T avg_cost
0000000000008200 T avg_cost_add
00000000000081e0 T avg_cost_clear
0000000000008190 T avg_cost_deinit
0000000000008100 T avg_cost_init
0000000000009730 T is_const
0000000000009710 T is_const_deinit
0000000000009680 T is_const_init
0000000000009320 T sql_sequence
…
```

### Loading and using

Copy the `.so` to `plugin_dir` (usually `/usr/lib/mysql/plugin/`), then:

```sql
CREATE FUNCTION sum_int RETURNS integer SONAME 'libmy_udf.so';
SELECT sum_int(1, 2, 3);
```

Testing in Docker is recommended so you don't disturb a host SQL installation — see the `udf-examples` README for the canonical Dockerfile.

### State of the art (2024)

`udf` for MariaDB/MySQL is the most mature Rust UDF framework across all engines. Comparisons:

- **`pgrx`** (Postgres) — comparable maturity for a different engine; richer surface (UDFs, types, BGWs, hooks).
- **`sqlite-loadable-rs`** (SQLite) — comparable design, different engine semantics ([[sqlite_loadable]]).
- **No equivalent for Snowflake / BigQuery / Spark / Hive** — the JVM/cloud engines don't have a Rust UDF SDK; you bridge via JNI ([[hive]], [[spark]]) or use ADBC for client-side Arrow ([[../data/adbc|ADBC]]).
