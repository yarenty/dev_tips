---
title: sql-parse — small MariaDB-focused SQL parser
main_link: https://crates.io/crates/sql-parse
keywords: [sql-parse, mariadb, mysql, parser, ast, no-std, recursive-descent]
status: reviewed
review_date: 2026/05/03
---

# sql-parse — small MariaDB-focused SQL parser

**Main link:** <https://crates.io/crates/sql-parse>

## Summary

`sql-parse` is a small Rust SQL parser by Jonas Rosendahl (used internally at his `sqlx-tips` work), focused on **MariaDB / MySQL** dialects (PostgreSQL has partial support). It is a hand-written recursive-descent parser with no dependencies, no_std + alloc compatible, `#![forbid(unsafe_code)]`, that produces a typed AST with byte-span annotations on every node — designed so errors and lint-style diagnostics can be presented with caret-style underlining at exact source positions.

## Insight

This is **not** the parser you reach for if you want broad dialect coverage or DataFusion compatibility — that's [[../data/sqlparser|sqlparser-rs]] (Apache fork at `apache/datafusion-sqlparser-rs`), which has dozens of dialects, hundreds of contributors, and is the de-facto Rust SQL parser. Reach for `sql-parse` instead when (a) you specifically need MariaDB/MySQL semantics with span-precise error reporting (linters, IDE-style diagnostics, formatters), (b) you want zero dependencies and no_std, or (c) you are doing static-analysis tooling where the recursive-descent error recovery (continues parsing past syntax errors) is more useful than sqlparser's strict parsing. Status check: the crate has had infrequent updates over the years — verify activity and bus factor on crates.io before depending on it for anything serious. The original 2021 notes here flagged this as "untouched for over a year" — the project has had subsequent activity, but the comparison still holds: sqlparser-rs is vastly more active.

## Similar / related topics

- [[../data/sqlparser|sqlparser-rs]] — the dominant Rust SQL parser (Apache fork, used by DataFusion).
- [[datafusion]] — DataFusion's planner sits on sqlparser-rs, not on this crate.
- **`logos`** — fast lexer-generator; useful for the tokenisation half if you're writing your own.
- **`pest` / `nom` / `winnow` / `chumsky`** — generic parser-combinator / PEG crates (see [[../io/parsers]]).

## Internal links

- [[README]]
- [[../data/sqlparser|sqlparser-rs]] — disambiguation: this is the *other* Rust SQL parser.
- [[datafusion]] — what most people end up wanting after they realise they need a planner too.
- [[../io/parsers|Rust parser landscape]]
- [[mariadb]] — the dialect this crate targets best.

## Keywords

`#sql-parse` `#mariadb` `#mysql` `#parser` `#ast` `#no-std` `#recursive-descent`

## References / raw notes

- Crate: <https://crates.io/crates/sql-parse>

### Properties

- Good error recovery — the parser continues after errors so multi-issue reports are possible.
- All AST nodes implement `Spanned` (byte spans into the source) — useful for caret diagnostics.
- No deps, no_std + alloc.
- `#![forbid(unsafe_code)]`.
- Hand-written recursive descent with O(1) shift-reduce for expression parsing.

### Example

```rust
use sql_parse::{SQLDialect, SQLArguments, ParseOptions, parse_statement};

let options = ParseOptions::new()
    .dialect(SQLDialect::MariaDB)
    .arguments(SQLArguments::QuestionMark)
    .warn_unquoted_identifiers(true);

let mut issues = Vec::new();

let sql = "SELECT `monkey`,
           FROM `t1` LEFT JOIN `t2` ON `t2`.`id` = `t1.two`
           WHERE `t1`.`id` = ?";

let ast = parse_statement(sql, &mut issues, &options);

println!("Issues: {issues:#?}");
println!("AST:    {ast:#?}");
```
