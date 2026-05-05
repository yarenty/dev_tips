---
title: rawsql
main_link: https://crates.io/crates/rawsql
keywords: [rawsql, sql-engine, rust, programming, sql, crates, comment]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# rawsql

**Main link:** <https://crates.io/crates/rawsql>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[programming/rust/sql_engine/sqlparser|sqlparser]] — SQLparser _(score 32.0)_
- [[spark]] — Spark UDF _(score 28.7)_
- [[snowflake]] — Snowflake _(score 28.7)_
- [[mariadb]] — MariaDB _(score 28.7)_
- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 23.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#rawsql` `#sql-engine` `#rust` `#programming` `#sql` `#crates` `#files` `#comment`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# rawsql

https://crates.io/crates/rawsql


A rust library for using and reusing SQL.

You can integrate rawsql into your project through the releases on crates.io:
```toml
# Cargo.toml
[dependencies]
rawsql = "0.1.1"
```

## Overview
You need to write SQL and you need to reuse it. You don't want to duplicate the queries all over the code. This lib is for you.

This lib does not execute any sql in the DB.

## Usage
The basic idea is to separate the sql part from the code and put it into its own sql files. With this approach, you gain sql powers and the ability to write sql only once that runs on your DB (your dba could modify these files too)

The sql file needs to be with this format :
```sql
-- name: insert-person
INSERT INTO "person" (name, data) VALUES ($1, $2);

-- name: select-persons
SELECT id, name, data FROM person;
```
comment with the **-- name: ** , it will be the key value for getting each query.

the ";" will be needed at the end of the query.
```rust

extern crate rawsql;

use rawsql::Loader;


fn main() {

    let queries = Loader::get_queries_from("examples/postgre.sql").unwrap().queries;

    //Insert query
    let qinsert = queries.get("insert-person").unwrap();

    println!("{}", qinsert);

    //Select query
    let qselect = queries.get("select-persons").unwrap();
    println!("{}", qselect);

}
```


In rust there is not yet a general driver like JDBC or go's database/sql so I decide to abstract first the parser of sql files to use directly with the libs already exists for each DB.

## Comment

This is quite strightforward.
Looking for comparison to snowflake UDF support - looks like almost enough.


## TODO

How to integrate with Datafusion?
Process $1,$2 as columns of arrow ?
