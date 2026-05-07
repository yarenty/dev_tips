---
title: MariaDB (and the Rust UDF angle)
main_link: https://mariadb.org/
keywords: [mariadb, mysql, udf, rust, sql-udf, columnstore]
status: reviewed
---

# MariaDB (and the Rust UDF angle)

**Main link:** <https://mariadb.org/>

## Summary

MariaDB is the community-maintained MySQL fork (forked by Monty Widenius after Oracle acquired Sun/MySQL in 2009). Like MySQL it is **C/C++**, not Rust — filed in this section historically because (a) the Rust UDF story for MariaDB/MySQL is by far the most polished in the wider Rust SQL-engine ecosystem (see [[udf_lib|`udf` crate]]), and (b) MariaDB is an interesting cross-section of the Rust + relational world via its **ColumnStore** engine (columnar) and **MariaDB SkySQL** managed offering. For the Rust *client* side, see [[../data/mysql|`mysql` / `mysql_async`]].

## Insight

The reason this article exists in the Rust section is `pluots/sql-udf` (better known by its crate name [[udf_lib|`udf`]]) — a Rust framework that lets you write MariaDB/MySQL UDFs as ordinary Rust structs implementing `BasicUdf` / `AggregateUdf`, compile to a `cdylib` `.so`, and load with `CREATE FUNCTION foo RETURNS … SONAME 'libfoo.so';`. As of 2024 this is **the most mature Rust UDF story across all SQL engines** — you have proper aggregate support, init/process/clear/add/remove lifecycle methods, the GAT-based trait design, and a Docker-based test harness. Reach for it when you want to push CPU-heavy logic (regex, vector math, custom string ops) into MariaDB without writing C. Gotchas: the binary needs to live in the server's `plugin_dir` (usually `/usr/lib/mysql/plugin/`); the calling convention is per-row not per-batch (no Arrow); and the same `.so` is **not** loadable into PostgreSQL — for that, you want `pgrx`. **Note**: the original MySQL UDF C ABI is shared between MySQL and MariaDB, so the same `.so` works on both, but recent MySQL versions have started to diverge — pin and test.

## Similar / related topics

- [[udf_lib|`udf` crate]] — the canonical Rust framework for MariaDB/MySQL UDFs (same author as the original notes here).
- [[../data/mysql|`mysql` / `mysql_async`]] — the Rust client-side story.
- **`pgrx`** — the Postgres equivalent: write Postgres extensions (incl. UDFs, types, hooks, BGWs) in Rust.
- [[sqlite_loadable]] — the SQLite equivalent: write loadable extensions in Rust.
- **MariaDB ColumnStore** — MariaDB's columnar engine (the analytical-workload story).

## Internal links

<!-- reviewed -->
- [[README]]
- [[udf_lib]] — the actually-Rust part of this story.
- [[../data/mysql|MySQL/MariaDB Rust client]]
- [[../../../db/relational/mysql|MySQL/MariaDB substrate]]
- [[../../../db/udf/external_udfs|external UDFs survey]]

## Keywords

`#mariadb` `#mysql` `#udf` `#rust` `#sql-udf` `#columnstore`

## References / raw notes

- MariaDB project home: <https://mariadb.org/>
- MariaDB UDF C-API reference: <https://mariadb.com/kb/en/user-defined-functions/>
- `pluots/sql-udf` (the Rust framework): <https://github.com/pluots/sql-udf>
- Working examples: <https://github.com/pluots/sql-udf/tree/main/udf-examples>

### Loading a Rust-built UDF into MariaDB

Once you have built `libudf_examples.so` and copied it into `plugin_dir`:

```sh
docker exec -it mariadb_udf_test mysql -pexample
```

```sql
CREATE FUNCTION is_const   RETURNS string  SONAME 'libudf_examples.so';
CREATE FUNCTION lookup6    RETURNS string  SONAME 'libudf_examples.so';
CREATE FUNCTION sum_int    RETURNS integer SONAME 'libudf_examples.so';
CREATE FUNCTION udf_sequence RETURNS integer SONAME 'libudf_examples.so';
CREATE FUNCTION lipsum     RETURNS string  SONAME 'libudf_examples.so';
CREATE FUNCTION log_calls  RETURNS integer SONAME 'libudf_examples.so';
CREATE AGGREGATE FUNCTION avg2       RETURNS real    SONAME 'libudf_examples.so';
CREATE AGGREGATE FUNCTION avg_cost   RETURNS real    SONAME 'libudf_examples.so';
CREATE AGGREGATE FUNCTION udf_median RETURNS integer SONAME 'libudf_examples.so';
```
