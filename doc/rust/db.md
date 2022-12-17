# diesel

https://crates.io/crates/diesel

Diesel - A safe, extensible ORM and Query Builder for Rust
Diesel is the most productive way to interact with databases in Rust because of its safe and composable abstractions over queries.


https://github.com/diesel-rs/diesel/tree/2.0.x


Diesel gets rid of the boilerplate for database interaction and eliminates runtime errors without sacrificing performance. It takes full advantage of Rust's type system to create a low overhead query builder that "feels like Rust."

Supported databases:

- PostgreSQL
- MySQL
  -SQLite

You can configure the database backend in Cargo.toml:

```toml
[dependencies]
diesel = { version = "<version>", features = ["<postgres|mysql|sqlite>"] }
```

## Getting Started
Find our extensive Getting Started tutorial at https://diesel.rs/guides/getting-started. Guides on more specific features are coming soon.

# r2d2


# surrealDB
[db->surrealDB](../db/surrealDB.md)

# mysql 
https://crates.io/crates/mysql

This crate offers:

- MySql database driver in pure rust;
- connection pool.

## Features:

- macOS, Windows and Linux support;
- TLS support via nativetls or rustls (see the SSL Support section);
- MySql text protocol support, i.e. support of simple text queries and text result sets;
- MySql binary protocol support, i.e. support of prepared statements and binary result sets;
- support of multi-result sets;
- support of named parameters for prepared statements (see the Named Parameters section);
- optional per-connection cache of prepared statements (see the Statement Cache section);
- buffer pool (see the Buffer Pool section);
- support of MySql packets larger than 2^24;
- support of Unix sockets and Windows named pipes;
- support of custom LOCAL INFILE handlers;
- support of MySql protocol compression;
- support of auth plugins:
  - mysql_native_password - for MySql prior to v8;
  - caching_sha2_password - for MySql v8 and higher.

# refinery 

https://crates.io/crates/refinery

Powerful SQL migration toolkit for Rust

# rusqlite

https://crates.io/crates/rusqlite

Rusqlite is an ergonomic wrapper for using SQLite from Rust.

# barrel

https://crates.io/crates/barrel

A powerful database schema builder, that lets you write your SQL migrations in Rust!

barrel offers callback-style builder functions for SQL migrations and is designed to be flexible, portable and fun to use. It provides you with a common interface over SQL, with additional database-specific builders.

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


# GlueSQL

https://crates.io/crates/gluesql


GlueSQL is a SQL database library written in Rust.
It provides a parser (sqlparser-rs), execution layer, and optional storages (sled or memory) packaged into a single library.
Developers can choose to use GlueSQL to build their own SQL database, or as an embedded SQL database using the default storage engine.



# sqlparser

https://crates.io/crates/sqlparser

The goal of this project is to build a SQL lexer and parser capable of parsing SQL that conforms with the ANSI/ISO SQL standard while also making it easy to support custom dialects so that this crate can be used as a foundation for vendor-specific parsers.

This parser is currently being used by the DataFusion query engine, LocustDB, Ballista and GlueSQL.

This crate provides only a syntax parser, and tries to avoid applying any SQL semantics, and accepts queries that specific databases would reject, even when using that Database's specific Dialect. For example, CREATE TABLE(x int, x int) is accepted by this crate, even though most SQL engines will reject this statement due to the repeated column name x.



# SeqQuery

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



