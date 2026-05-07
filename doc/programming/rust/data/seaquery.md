---
title: SeaQuery — dynamic SQL query builder
main_link: https://crates.io/crates/sea-query
keywords: [seaquery, sea-orm, query-builder, sql, postgres, mysql, sqlite]
status: reviewed
---

# SeaQuery — dynamic SQL query builder

**Main link:** <https://crates.io/crates/sea-query>

## Summary

`sea-query` is a dynamic SQL query builder for MySQL, PostgreSQL, and SQLite, part of the SeaQL family. You construct queries (selects, inserts, updates, deletes, schema DDL, foreign keys, indexes) as ASTs and `.build()` them against a per-backend `QueryBuilder` to get parameterised SQL + values. It integrates with `sqlx`, `postgres`, and `rusqlite` for execution. It is the foundation of `sea-orm` (the SeaQL async ORM) but is perfectly usable on its own.

## Insight

The Rust SQL-API spectrum has three poles: **`diesel`** (full ORM with compile-time-checked schema via macros — the most type-safe, the most opinionated), **`sqlx`** (raw SQL strings + compile-time check against a live DB — minimal abstraction), and the **builder** group (`sea-query`, `welds`'s expression layer, hand-rolled `format!`). `sea-query` is the canonical builder: pick it when you need to construct queries dynamically (admin UIs, generic filters, SaaS multi-tenancy where the WHERE clauses depend on user input) and you don't want to either (a) write a stringly-typed `format!` template (injection risk) or (b) bring in a full ORM. Pair with `sqlx` for execution and you get the best of both worlds: dynamic builder + compile-time-checked execution. Gotchas: `sea-query`'s `Iden` enum boilerplate gets verbose (use the `derive` feature); migrations are owned by `sea-schema`/`sea-orm-migration`, not `sea-query` itself.

## Similar / related topics

- `diesel` — full ORM with macro-generated schema; type-safest of the three poles.
- `sqlx` — raw SQL + compile-time check; lowest abstraction.
- `sea-orm` — SeaQL's async ORM, built on top of `sea-query`.
- `welds` — async ORM with its own expression layer.

## Internal links
- [[programming/rust/sql_engine/diesel|diesel]]
- [[programming/rust/sql_engine/seaquery|sea-query (sql_engine view)]]
- [[barrel]] / [[refinery]] — schema migrations
- [[sqlparser]]

## Keywords

`#seaquery` `#sea-orm` `#query-builder` `#sql` `#postgres` `#mysql` `#sqlite`

## References / raw notes

- Crate: <https://crates.io/crates/sea-query>
- Repo: <https://github.com/SeaQL/sea-query>

Backend support: MySQL, PostgreSQL, SQLite. Execution integrations: `sqlx`, `postgres`, `rusqlite`.

Feature flags worth knowing:

- `derive` — `#[derive(Iden)]` to skip the manual enum boilerplate.
- `backend-mysql` / `backend-postgres` / `backend-sqlite` — pick your dialects.
- `with-chrono` / `with-time` / `with-uuid` / `with-json` / `with-rust_decimal` / `with-bigdecimal` / `with-ipnetwork` / `with-mac_address` / `postgres-array` / `postgres-interval` — type-mapping adapters.

Two example use cases that justify the builder over raw SQL:

```rust
// 1. Parameterised IN clause without manually counting placeholders.
let (sql, values) = Query::select()
    .column(Glyph::Image)
    .from(Glyph::Table)
    .and_where(Expr::col(Glyph::Image).like("A"))
    .and_where(Expr::col(Glyph::Id).is_in([1, 2, 3]))
    .build(PostgresQueryBuilder);
// SELECT "image" FROM "glyph" WHERE "image" LIKE $1 AND "id" IN ($2, $3, $4)

// 2. Conditionally added clauses.
Query::select()
    .column(Char::Character)
    .from(Char::Table)
    .conditions(
        user_filter_active,
        |q| { q.and_where(Expr::col(Char::Id).eq(1)); },
        |_| {},
    );
```

Capabilities: SELECT/INSERT/UPDATE/DELETE, aggregates, casting, custom functions, CTEs; schema DDL: `Table::create/alter/drop/rename/truncate`, `ForeignKey::create/drop`, `Index::create/drop`.
