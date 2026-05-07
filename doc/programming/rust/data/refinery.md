---
title: refinery — embedded SQL migration runner
main_link: https://crates.io/crates/refinery
keywords: [refinery, migration, sql, postgres, mysql, sqlite, mssql]
status: reviewed
---

# refinery — embedded SQL migration runner

**Main link:** <https://crates.io/crates/refinery>

## Summary

`refinery` is an embedded SQL migration runner inspired by Java's Flyway. You write `V<n>__<name>.sql` files (or Rust `Migration::unapplied` programs), point refinery at a connection, and it tracks applied versions in a `refinery_schema_history` table. Driver coverage is good: PostgreSQL (`tokio-postgres`, `postgres`), MySQL (`mysql`, `mysql_async`), SQLite (`rusqlite`), and MSSQL (`tiberius`). Async + sync flavours are both supported via feature flags.

## Insight

Reach for `refinery` when you want a thin, no-drama, Flyway-style migration runner that doesn't require a particular ORM. The decision tree: **`diesel-migrations`** if your app is built on Diesel; **`sea-orm-migration`** if it's built on SeaORM; **`sqlx-cli migrate`** if it's built on sqlx (and you don't mind the binary on the deploy host); **`refinery`** for everything else. Refinery's killer feature is being **embedded** — your CLI/server can run pending migrations on startup with a few lines, no separate `migrate` binary needed. Gotchas: rollback ("undo") is intentionally not supported (Flyway parity, considered an antipattern); the `embed_migrations!` macro reads files at compile time so the binary ships its migrations baked in (this is usually what you want); be careful with backend-specific dialect — refinery does not abstract SQL.

## Similar / related topics

- [[barrel]] — Rust DSL for *generating* schema SQL; pair with refinery to run it.
- `diesel-migrations` — Diesel's bundled runner.
- `sea-orm-migration` — SeaQL's runner with a builder DSL.
- `sqlx-cli migrate` — sqlx's runner; comparable feature set.
- Flyway / Liquibase / Alembic / Atlas — non-Rust prior art.

## Internal links
- [[barrel]]
- [[programming/rust/sql_engine/diesel|diesel]]
- [[rusqlite]]
- [[mysql]]
- [[tiberius]]

## Keywords

`#refinery` `#migration` `#sql` `#postgres` `#sqlite` `#mssql`

## References / raw notes

- Crate: <https://crates.io/crates/refinery>
- Repo: <https://github.com/rust-db/refinery>

> Powerful SQL migration toolkit for Rust.

Supported drivers (feature-gated): `tokio-postgres`, `postgres`, `mysql`, `mysql_async`, `rusqlite`, `tiberius`.

Typical workflow:

```rust
use refinery::embed_migrations;

embed_migrations!("./migrations");

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let mut conn = postgres::Client::connect("...", postgres::NoTls)?;
    migrations::runner().run(&mut conn)?;
    Ok(())
}
```

Migration files in `./migrations/V1__init.sql`, `V2__add_users.sql`, etc.
