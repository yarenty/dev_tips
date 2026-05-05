---
title: SQLparser
main_link: https://crates.io/crates/sqlparser
keywords: [sqlparser, rust, sql]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# SQLparser

**Main link:** <https://crates.io/crates/sqlparser>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 39.6)_
- [[datafusion]] — Datafusion SQL Query Planner _(score 34.7)_
- [[rawsql]] — rawsql _(score 32.0)_
- [[gluesql]] — GlueSQL _(score 23.0)_
- [[rtic]] — RTIC _(score 18.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#sqlparser` `#sql-engine` `#rust` `#programming` `#sql` `#datafusion` `#crate` `#parser`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# SQLparser

https://crates.io/crates/sqlparser

! USED by Datafusion !

The goal of this project is to build a SQL lexer and parser capable of parsing SQL that conforms with the ANSI/ISO SQL standard while also making it easy to support custom dialects so that this crate can be used as a foundation for vendor-specific parsers.

This parser is currently being used by the DataFusion query engine, LocustDB, Ballista and GlueSQL.

This crate provides only a syntax parser, and tries to avoid applying any SQL semantics, and accepts queries that specific databases would reject, even when using that Database's specific Dialect. For example, CREATE TABLE(x int, x int) is accepted by this crate, even though most SQL engines will reject this statement due to the repeated column name x.


This crate avoids semantic analysis because it varies drastically between dialects and implementations. If you want to do semantic analysis, feel free to use this project as a base

## Example
To parse a simple SELECT statement:
```rust
use sqlparser::dialect::GenericDialect;
use sqlparser::parser::Parser;

let sql = "SELECT a, b, 123, myfunc(b) \
FROM table_1 \
WHERE a > b AND b < 100 \
ORDER BY a DESC, b";

let dialect = GenericDialect {}; // or AnsiDialect, or your own dialect ...

let ast = Parser::parse_sql(&dialect, sql).unwrap();

println!("AST: {:?}", ast);
```
This outputs
```log
AST: [Query(Query { ctes: [], body: Select(Select { distinct: false, projection: [UnnamedExpr(Identifier("a")), UnnamedExpr(Identifier("b")), UnnamedExpr(Value(Long(123))), UnnamedExpr(Function(Function { name: ObjectName(["myfunc"]), args: [Identifier("b")], over: None, distinct: false }))], from: [TableWithJoins { relation: Table { name: ObjectName(["table_1"]), alias: None, args: [], with_hints: [] }, joins: [] }], selection: Some(BinaryOp { left: BinaryOp { left: Identifier("a"), op: Gt, right: Identifier("b") }, op: And, right: BinaryOp { left: Identifier("b"), op: Lt, right: Value(Long(100)) } }), group_by: [], having: None }), order_by: [OrderByExpr { expr: Identifier("a"), asc: Some(false) }, OrderByExpr { expr: Identifier("b"), asc: None }], limit: None, offset: None, fetch: None })]
```
