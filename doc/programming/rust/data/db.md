---
title: diesel — Rust ORM and query builder
main_link: https://diesel.rs/
keywords: [diesel, orm, query-builder, postgres, mysql, sqlite, sql]
status: reviewed
---

# diesel — Rust ORM and query builder

**Main link:** <https://diesel.rs/>

## Summary

Diesel is the most established ORM and query builder in Rust, by Sean Griffin. It uses procedural macros to generate a typed schema from your migrations (or from `diesel print-schema`), then exposes a strongly-typed query builder where most query mistakes (missing columns, wrong types, missing joins) become compile-time errors. Backends: PostgreSQL, MySQL, SQLite. The 2.x release line shipped in 2022; the async story is `diesel-async` (2024+).

This file was historically the multi-topic parent for the entire `data/` section. The split-out children — driver/client crates, connection pools, query builders, migration runners, embedded engines — each now have their own article. See the parent README for the index.

## Insight

Reach for diesel when (a) you want **maximum compile-time safety** in your DB code and don't mind macro-heavy build times, (b) your schema is reasonably stable, (c) you're OK with the (still maturing) async story or are happy with sync. Position vs alternatives: **`sqlx`** trades macro-generated schema for runtime SQL strings + offline-checked queries against a live DB (less type-safety, more flexibility, simpler mental model — the modern default for new projects); **`sea-orm`** is a more dynamic ORM around `sea-query` (good for dynamic queries / admin UIs); **`welds`** is async-first, simpler. Diesel's killer feature remains **the schema-as-types** pattern — `users::name` is a real type, not a string. Gotchas: macro errors are notoriously cryptic — keep `diesel.toml` clean and re-run `diesel print-schema` after every migration; the MySQL backend uses C `mysqlclient`, the SQLite backend uses C `libsqlite3`, only the Postgres backend ships pure-Rust by default; for connection pooling use `diesel::r2d2` (sync) or `diesel-async` + `bb8`/`deadpool` (async).

## Similar / related topics

- [[seaquery]] — query-builder-only sibling (no ORM); SeaQL family.
- `sqlx` — raw SQL with compile-time check; the modern default for new code.
- `sea-orm` — async ORM atop `sea-query`.
- `welds` — async-first ORM.
- [[r2d2]] / [[bb8]] / [[deadpool]] — pooling.
- [[refinery]] / [[barrel]] — migrations (also `diesel-migrations` built into Diesel).

## Internal links
- [[programming/rust/sql_engine/diesel|diesel (sql_engine view)]]
- [[seaquery]]
- [[mysql]]
- [[rusqlite]]
- [[r2d2]]
- [[refinery]]

## Keywords

`#diesel` `#orm` `#query-builder` `#postgres` `#mysql` `#sqlite`

## References / raw notes

- Site: <https://diesel.rs/>
- Crate: <https://crates.io/crates/diesel>
- Repo: <https://github.com/diesel-rs/diesel>
- Async sibling: <https://crates.io/crates/diesel-async>
- Getting Started: <https://diesel.rs/guides/getting-started>
- Talk: ["Diesel — A safe, extensible ORM and Query Builder for Rust"](https://www.youtube.com/watch?v=tRC4EIKhMzw)

```toml
[dependencies]
diesel = { version = "2", features = ["postgres"] }   # or "mysql" / "sqlite"
```

This article previously hosted the multi-topic dump that was split into the per-tool siblings now in `programming/rust/data/`. Refer to the section README for the canonical index of those children.
