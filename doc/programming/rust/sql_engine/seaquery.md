# SeaQuery

https://crates.io/crates/sea-query

🔱 A dynamic query builder for MySQL, Postgres and SQLite


SeaQuery is a query builder to help you construct dynamic SQL queries in Rust. You can construct expressions, queries and schema as abstract syntax trees using an ergonomic API. We support MySQL, Postgres and SQLite behind a common interface that aligns their behaviour where appropriate.

We provide integration for SQLx, postgres and rusqlite. See examples for usage.

SeaQuery is the foundation of SeaORM, an async & dynamic ORM for Rust.


SeaQuery is very lightweight, all dependencies are optional.

## Feature flags
- Macro: derive attr
- Async support: thread-safe (use Arc inplace of Rc)
- SQL engine: backend-mysql, backend-postgres, backend-sqlite
- Type support: with-chrono, with-time, with-json, with-rust_decimal, with-bigdecimal, with-uuid, with-ipnetwork, with-mac_address, postgres-array, postgres-interval

## Usage

- Basics
  - Iden
  - Expression
  - Condition
  - Statement Builders
- Query Statement
  - Query Select
  - Query Insert
  - Query Update
  - Query Delete
- Advanced
  - Aggregate Functions
  - Casting
  - Custom Function
- Schema Statement
  - Table Create
  - Table Alter
  - Table Drop
  - Table Rename
  - Table Truncate
  - Foreign Key Create
  - Foreign Key Drop
  - Index Create
  - Index Drop

## Motivation
Why would you want to use a dynamic query builder?

### Parameter bindings
One of the headaches when using raw SQL is parameter binding. With SeaQuery you can:

```rust
assert_eq!(
    Query::select()
        .column(Glyph::Image)
        .from(Glyph::Table)
        .and_where(Expr::col(Glyph::Image).like("A"))
        .and_where(Expr::col(Glyph::Id).is_in([1, 2, 3]))
        .build(PostgresQueryBuilder),
    (
        r#"SELECT "image" FROM "glyph" WHERE "image" LIKE $1 AND "id" IN ($2, $3, $4)"#
            .to_owned(),
        Values(vec![
            Value::String(Some(Box::new("A".to_owned()))),
            Value::Int(Some(1)),
            Value::Int(Some(2)),
            Value::Int(Some(3))
        ])
    )
);

```

### Dynamic query
You can construct the query at runtime based on user inputs:
```rust
Query::select()
    .column(Char::Character)
    .from(Char::Table)
    .conditions(
        // some runtime condition
        true,
        // if condition is true then add the following condition
        |q| {
            q.and_where(Expr::col(Char::Id).eq(1));
        },
        // otherwise leave it as is
        |q| {},
    );
```


[more](https://crates.io/crates/sea-query)