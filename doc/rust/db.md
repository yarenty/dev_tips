# diesel

https://crates.io/crates/diesel


## youtube
https://www.youtube.com/watch?v=tRC4EIKhMzw


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

https://github.com/sfackler/r2d2



A generic connection pool for Rust.

Documentation

Opening a new database connection every time one is needed is both inefficient and can lead to resource exhaustion under high traffic conditions. A connection pool maintains a set of open connections to a database, handing them out for repeated use.

r2d2 is agnostic to the connection type it is managing. Implementors of the ManageConnection trait provide the database-specific logic to create and check the health of connections.

A (possibly not exhaustive) list of adaptors for different backends:

| Backend |	Adaptor Crate|
|------|-----|
|rust-postgres	|r2d2-postgres
|redis-rs	|use r2d2 feature of redis-rs
|rust-memcache	|r2d2-memcache
|rust-mysql-simple	|r2d2-mysql
|rusqlite	|r2d2-sqlite
|rsfbclient	|r2d2-firebird
|rusted-cypher	|r2d2-cypher
|diesel	|diesel::r2d2 (docs)
|couchdb	|r2d2-couchdb
|mongodb (archived)	| r2d2-mongodb (deprecated: official driver handles pooling internally)
|odbc	|r2d2-odbc
|jfs	|r2d2-jfs
|oracle	|r2d2-oracle
|ldap3	|r2d2-ldap
|duckdb-rs	|use r2d2 feature of duckdb-rs

## Example
Using an imaginary "foodb" database.

```rust
use std::thread;

extern crate r2d2;
extern crate r2d2_foodb;

fn main() {
    let manager = r2d2_foodb::FooConnectionManager::new("localhost:1234");
    let pool = r2d2::Pool::builder()
        .max_size(15)
        .build(manager)
        .unwrap();

    for _ in 0..20 {
        let pool = pool.clone();
        thread::spawn(move || {
            let conn = pool.get().unwrap();
            // use the connection
            // it will be returned to the pool when it falls out of scope.
        })
    }
}
```






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




# tiberius

https://crates.io/crates/tiberius


A native Microsoft SQL Server (TDS) client for Rust.

## Goals
- A perfect implementation of the TDS protocol.
- Asynchronous network IO.
- Independent of the network protocol.
- Support for latest versions of Linux, Windows and macOS.
## Non-goals
- Connection pooling (use bb8, mobc, deadpool or any of the other asynchronous connection pools)
- Query building
- Object-relational mapping


# deadpool

https://crates.io/crates/deadpool

Deadpool is a dead simple async pool for connections and objects of any type.

This crate provides two implementations:

Managed pool (deadpool::managed::Pool)

Creates and recycles objects as needed
Useful for database connection pools
Enabled via the managed feature in your Cargo.toml
Unmanaged pool (deadpool::unmanaged::Pool)

All objects either need to be created by the user and added to the pool manually. It is also possible to create a pool from an existing collection of objects.
Enabled via the unmanaged feature in your Cargo.toml


# mobc

https://crates.io/crates/mobc

A generic connection pool with async/await support.

Inspired by r2d2 and Golang SQL package.


# bb8

https://crates.io/crates/bb8

full-featured connection pool, designed for asynchronous connections (using tokio). Originally based on r2d2.

Opening a new database connection every time one is needed is both inefficient and can lead to resource exhaustion under high traffic conditions. A connection pool maintains a set of open connections to a database, handing them out for repeated use.

bb8 is agnostic to the connection type it is managing. Implementors of the ManageConnection trait provide the database-specific logic to create and check the health of connections.

A (possibly not exhaustive) list of adapters for different backends:

| Backend |	Adapter Crate |
|-----|-----|
| tokio-postgres	| bb8-postgres (in-tree)
| redis	| bb8-redis (in-tree)
| rsmq	| rsmq_async
| bolt-client	| bb8-bolt
| diesel	| bb8-diesel
| tiberius	| bb8-tiberius
| nebula-client	| bb8-nebula
| memcache-async	| bb8-memcached
| lapin	| bb8-lapin



# GreptimeDB

https://github.com/GreptimeTeam/greptimedb

GreptimeDB is an open-source time-series database with a special focus on scalability, analytical capabilities and efficiency. It's designed to work on infrastructure of the cloud era, and users benefit from its elasticity and commodity storage.

Our core developers have been building time-series data platform for years. Based on their best-practices, GreptimeDB is born to give you:

A standalone binary that scales to highly-available distributed cluster, providing a transparent experience for cluster users
Optimized columnar layout for handling time-series data; compacted, compressed, stored on various storage backends
Flexible index options, tackling high cardinality issues down
Distributed, parallel query execution, leveraging elastic computing resource
Native SQL, and Python scripting for advanced analytical scenarios
Widely adopted database protocols and APIs
Extensible table engine architecture for extensive workloads


# parseable


https://github.com/parseablehq/parseable
Parseable is a lightweight, cloud native log observability engine. It can use either a local drive or S3 (and compatible stores) for backend data storage.

Parseable is written in Rust and uses Apache Arrow and Parquet as underlying data structures. Additionally, it uses a simple, index-free mechanism to organize and query data allowing low latency, and high throughput ingestion and query.

Parseable consumes up to ~80% lower memory and ~50% lower CPU than Elastic for similar ingestion throughput.

Parseable UI Demo (Credentials: parseable,parseable) ↗︎
Grafana Dashboard Demo ↗︎
🚀 Features
Choose your own storage backend - local drive or S3 (or compatible) object store.
Ingestion API compatible with HTTP + JSON output of log agents - Fluentbit ↗︎, Vector ↗︎, Logstash ↗︎ and others.
Query log data with PostgreSQL compatible SQL.
Grafana ↗︎ for visualization.
Auto schema inference (schema evolution coming soon ↗︎).
Send alerts ↗︎ to webhook targets including Slack.
Stats API ↗︎ to track ingestion and compressed data.
Single binary includes all components - ingestion, store and query. Built-in UI.



# WooriDB

https://github.com/naomijub/wooridb

WooriDB is a general purpose (EXPERIMENTAL) time serial database, which means it contains all entities registries indexed by DateTime. It is schemaless, key-value storage and uses its own query syntax that is similar to SparQL and Crux's Datalog.

Some other features are:

- Hashing keys content with ENCRYPT keyword.
- Hashed values are filtered out and can only be checked with CHECK keyword.
- Ron schemas for input and output.
- JSON is supported via feature.
- Entities are indexed by entity_name (Entity Tree), DateTime (Time Serial) and Uuid (Entity ID). Entity format is a HashMap where keys are strings and values are supported Types.
- Stores persistent data locally.
- Able to handle very large numbers when using the P suffix.  Ex: 98347883122138743294728345738925783257325789353593473247832493483478935673.9347324783249348347893567393473247832493483478935673P.
- Configuration is done via environment variables.
- Authentication and Authorization via session token
- Conditional Update
- Some Relation Algebra
- Entity history

Woori means our and although I developed this DB initially alone, it is in my culture to call everything that is done for our community and by our community ours.

This project is hugely inspired by:

- Crux;
- Datomic;
- Prometheus
- SparQL.
- Database Internals
- Database System Concept
- Designing Data Intensive Application
- Professor Andy Pavlo Database classes.
- Zero Trust in Time Series Data?




# ADBC !


https://github.com/apache/arrow-adbc/tree/main



https://github.com/apache/arrow-adbc/blob/main/rust/src/lib.rs

ADBC: Arrow Database Connectivity

ADBC is an API standard (version 1.0.0) for database access libraries ("drivers") in C, Go, and Java that uses Arrow for result sets and query parameters. Instead of writing code to convert to and from Arrow data for each individual database, applications can build against the ADBC APIs, and link against drivers that implement the standard. Additionally, a JDBC/ODBC-style driver manager is provided. This also implements the ADBC APIs, but dynamically loads drivers and dispatches calls to them.

Like JDBC/ODBC, the goal is to provide a generic API for multiple databases. ADBC, however, is focused on bulk columnar data retrieval and ingestion through an Arrow-based API rather than attempting to replace JDBC/ODBC in all use cases. Hence, ADBC is complementary to those existing standards.

Like Arrow Flight SQL, ADBC is an Arrow-based way to work with databases. However, Flight SQL is a protocol defining a wire format and network transport as opposed to an API specification. Flight SQL requires a database to specifically implement support for it, while ADBC is a client API specification for wrapping existing database protocols which could be Arrow-native or not. Together, ADBC and Flight SQL offer a fully Arrow-native solution for clients and database vendors.

For more about ADBC, see the introductory blog post.

Status
ADBC versions the API standard and the implementing libraries separately.

The API standard (version 1.0.0) is considered stable, but enhancements may be made.

Libraries are under development. For more details, see the documentation.

