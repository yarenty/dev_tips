---
title: sqlparser-rs — ANSI/dialect SQL parser
main_link: https://crates.io/crates/sqlparser
keywords: [sqlparser, sql, parser, datafusion, ballista, ast]
status: reviewed
---

# sqlparser-rs — ANSI/dialect SQL parser

**Main link:** <https://crates.io/crates/sqlparser>

## Summary

`sqlparser-rs` is the canonical Rust crate for lexing and parsing SQL into an AST. Originally authored by Andy Grove (DataFusion / Ballista lineage), it follows the ANSI/ISO SQL standard while exposing a `Dialect` trait so vendor-specific extensions (Postgres `::` casts, MySQL backticks, MSSQL bracketed identifiers, Snowflake/BigQuery/Hive/Redshift/Databricks specifics) can be plugged in. It is the SQL frontend used by Apache DataFusion, LocustDB, Ballista, and historically GlueSQL — making it effectively the de-facto "Rust SQL parser."

## Insight

Reach for `sqlparser-rs` whenever you need to **inspect, rewrite, or route SQL** without executing it: query rewriting in a proxy/router, lint rules, schema-diff tools, dialect translators, code-mod from one warehouse to another, query-redaction for logs, etc. It is *only* a parser — it intentionally does no semantic validation, so `CREATE TABLE(x int, x int)` happily round-trips through it. Pair with DataFusion's `LogicalPlanBuilder` or your own analyzer when you actually need name resolution. Important fork note: in **2024 Apache DataFusion forked the crate** to `apache-datafusion-sqlparser-rs` (still on crates.io as `sqlparser`, but the canonical home moved to apache/datafusion-sqlparser-rs). The original `andygrove/sqlparser-rs` is the same project pre-fork; the Apache-hosted fork is the one to track for new dialects.

## Similar / related topics

- Apache DataFusion — primary downstream user of the parser.
- Ballista — distributed DataFusion sibling, also uses sqlparser.
- GlueSQL — uses sqlparser as its frontend.
- `sqlparser` (Python, via `sqlglot`) — comparable Python project; sqlglot is more aggressive about cross-dialect transpiling.
- `pg_query` (libpg_query bindings) — when you need 100% Postgres-grammar fidelity.

## Internal links
<!-- reviewed -->
- [[gluesql]]
- [[programming/rust/sql_engine/sqlparser|sqlparser (sql_engine view)]]
- [[datafusion/README|DataFusion]]
- [[seaquery]]

## Keywords

`#sqlparser` `#sql` `#parser` `#ast` `#datafusion`

## References / raw notes

- Crate: <https://crates.io/crates/sqlparser>
- Original repo: <https://github.com/sqlparser-rs/sqlparser-rs>
- Apache fork (2024): <https://github.com/apache/datafusion-sqlparser-rs>

The goal of this project is to build a SQL lexer and parser capable of parsing SQL that conforms to the ANSI/ISO SQL standard while making it easy to support custom dialects so that the crate can be used as a foundation for vendor-specific parsers. Currently used by DataFusion, LocustDB, Ballista, GlueSQL, and many more.

This crate provides only a syntax parser — it does not apply SQL semantics, and accepts queries that specific databases would reject. For example, `CREATE TABLE(x int, x int)` is accepted even though most engines reject duplicate column names.
