---
title: barrel — schema-builder DSL
main_link: https://crates.io/crates/barrel
keywords: [barrel, migration, schema, dsl, postgres, mysql, sqlite]
status: reviewed
---

# barrel — schema-builder DSL

**Main link:** <https://crates.io/crates/barrel>

## Summary

`barrel` is a small Rust DSL for writing database schema definitions (CREATE TABLE / ALTER / DROP / indexes / foreign keys) and rendering them to backend-specific SQL via per-dialect codegen (`Pg`, `Sqlite`, `MySql`, `MsSql`). It is not a migration runner — it produces SQL strings, which you then execute via your driver of choice or feed into a runner like `refinery` or `diesel-migrations`.

## Insight

Reach for `barrel` when you want to write your schema once in Rust and target multiple backends, or when you want to compose schema fragments programmatically (think: generating per-tenant tables in a multi-tenant SaaS). For most apps the simpler choice is **`refinery`** (raw `.sql` files + a runner) or **`diesel-migrations`** (the runner married to Diesel's macro-checked schema), or **`sea-orm-migration`** (the SeaQL equivalent — also DSL-flavoured but tighter integration with SeaORM). Gotchas: barrel's last 1.x release is from 2021 — it's stable but feature-frozen; non-trivial DDL (CHECK constraints, CREATE EXTENSION, partial indexes, generated columns, partitioning) generally falls back to inline raw SQL strings.

## Similar / related topics

- [[refinery]] — embedded migration runner; raw `.sql` files.
- `diesel-migrations` — Diesel's bundled migration tool (raw SQL or Rust).
- `sea-orm-migration` — SeaQL's migration runner with a builder DSL.
- `sqlx-cli migrate` — sqlx's bundled file-based migration runner.
- `atlas` (Go) — language-agnostic schema-as-code, popular alongside Rust services.

## Internal links
- [[refinery]]
- [[seaquery]]
- [[programming/rust/sql_engine/diesel|diesel]]
- [[rusqlite]]

## Keywords

`#barrel` `#migration` `#schema` `#dsl` `#postgres` `#mysql` `#sqlite`

## References / raw notes

- Crate: <https://crates.io/crates/barrel>
- Repo: <https://git.irde.st/spacekookie/barrel>

> A powerful database schema builder, that lets you write your SQL migrations in Rust!

```rust
use barrel::{types, Migration};
use barrel::backend::Pg;

fn main() {
    let mut m = Migration::new();

    m.create_table("users", |t| {
        t.add_column("name", types::varchar(255));
        t.add_column("age", types::integer());
        t.add_column("owns_plushy_sharks", types::boolean());
    });

    println!("{}", m.make::<Pg>());
}
```
