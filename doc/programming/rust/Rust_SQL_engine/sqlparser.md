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

