---
title: rawsql — load named SQL snippets from files
main_link: https://crates.io/crates/rawsql
keywords: [rawsql, sql, sql-files, query-loading, rust]
status: reviewed
---

# rawsql — load named SQL snippets from files

**Main link:** <https://crates.io/crates/rawsql>

## Summary

`rawsql` is a tiny Rust crate (last release 2018, more or less abandoned) that does one thing: loads a SQL file containing multiple named queries (delimited by `-- name: foo` comments and `;` terminators) into a `HashMap<String, String>` you can look up at runtime. It does **not** execute the SQL, doesn't parse it semantically, and doesn't bind parameters — it's a glorified string loader. Inspired by Python's `anosql` / Yesql, predating Rust's compile-time SQL ecosystem.

## Insight

For new code in 2025 this is **not** what you want. The modern Rust answer to "I have raw SQL, I want to keep it organised and execute it safely" is `sqlx::query!` / `query_file!` macros (compile-time-checked SQL against a real database) or [[../data/sqlparser|sqlparser-rs]] for parse-and-rewrite scenarios; both are vastly more capable. `rawsql` is interesting only as an example of the "SQL files separate from Rust code" pattern from the Diesel-versus-raw-SQL debate days, and as a pointer for legacy codebases that already use it. If you genuinely just want "named SQL snippets in files, no compile-time check, multi-DB-driver-agnostic loader" and don't want a 200kB dependency for it, write the 30 lines yourself.

## Similar / related topics

- **`sqlx`** with `query_file!` / `query_file_as!` — compile-time checked named SQL files; the canonical modern answer.
- **`anosql`** (Python), **Yesql** (Clojure) — the original "SQL in `.sql` files, looked up by name" pattern this crate ports.
- [[../data/sqlparser|sqlparser-rs]] — when you want to *parse and rewrite* the SQL, not just load strings.
- **`include_str!`** macro — for two-or-three-query projects, `let sql = include_str!("queries.sql");` plus a hand-rolled split is genuinely simpler.

## Internal links

- [[README]]
- [[../data/sqlparser|sqlparser-rs]] — the actual SQL-aware crate.
- [[../data/seaquery|SeaQuery]] — dynamic query builder; the alternative if you don't want raw SQL strings at all.
- [[datafusion]] — for building queries inside an engine.

## Keywords

`#rawsql` `#sql-files` `#query-loading` `#rust` `#legacy`

## References / raw notes

- Crate: <https://crates.io/crates/rawsql>
- Source: <https://github.com/cellularmitosis/rawsql> (last touched 2018)

### Format

```sql
-- name: insert-person
INSERT INTO "person" (name, data) VALUES ($1, $2);

-- name: select-persons
SELECT id, name, data FROM person;
```

### Usage

```rust
extern crate rawsql;
use rawsql::Loader;

fn main() {
    let queries = Loader::get_queries_from("examples/postgre.sql").unwrap().queries;

    let qinsert = queries.get("insert-person").unwrap();
    println!("{qinsert}");

    let qselect = queries.get("select-persons").unwrap();
    println!("{qselect}");
}
```

### Original questions in the notes

- *"How to integrate with DataFusion?"* — you wouldn't; DataFusion's planner takes SQL strings directly, the loader pattern adds nothing.
- *"Process `$1`, `$2` as columns of arrow?"* — no; that conflates positional parameters (driver-side bindings) with column references (logical plan). Use [[datafusion|`datafusion-sql`]] for the latter.
