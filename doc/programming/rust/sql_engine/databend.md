---
title: Databend
main_link: https://databend.rs/
keywords: [databend, rust, data-warehouse, snowflake-alternative, clickhouse-alternative, bsl, udf]
status: reviewed
---

# Databend

**Main link:** <https://databend.rs/>

## Summary

Databend is a Rust-built modern data warehouse, broadly Snowflake-shaped: separation of storage (S3 / object store) and compute, columnar engine, SQL frontend, with both an open-source single-binary deployment and a managed service ([Databend Cloud](https://databend.com/)). It speaks the **MySQL wire protocol** (so any MySQL client just works) and supports SQL-defined UDFs as well as Python UDFs via embedded interpreters. Licence is **Apache 2.0** for the core engine; the cloud product is the commercial story.

## Insight

Databend belongs to the "Rust single-binary infra rewrite" trend (alongside Redpanda / GreptimeDB / OpenObserve / Quickwit / Parseable) — the value proposition is "we replaced the JVM-heavy mishmash of Hive/Trino/Presto with one Rust binary that scales out". Reach for it when you want a Snowflake-shaped warehouse you can self-host on cheap object storage, with SQL semantics close enough to Postgres/MySQL that BI tools work out of the box. The most realistic comparisons are: **ClickHouse** (more mature, C++, single-node killer; Databend wins on cloud-native architecture and elastic compute); **DuckDB** (single-process embedded; not the same product); **StarRocks / Doris** (also Snowflake-shaped, C++); **Snowflake** itself (managed, much more polished, vastly more expensive). The UDF model is intentionally simple: SQL lambda-style for one-liners, Python for everything else; if you want compiled-Rust UDFs in this section's sense, you're closer to [[udf_lib|MySQL UDFs]] or [[sqlite_loadable|SQLite extensions]] than to Databend's design. **Watch the licence** — Databend's source is Apache 2.0, but the company has a separate enterprise edition; the *Databend Cloud* offering does not "double licence" the core in a hostile way (unlike, say, Redis 2024), but the trajectory is worth tracking.

## Similar / related topics

- **ClickHouse** — the C++ OLAP DB Databend is most often compared against.
- **StarRocks / Apache Doris** — MPP analytical SQL DBs in the same niche.
- **Snowflake** — the commercial reference design for separation-of-storage-and-compute warehouses.
- **DuckDB** — single-process columnar SQL; different deployment model, similar query semantics.
- **GreptimeDB** — Rust-built; time-series-focused cousin in the same trend.

## Internal links

- [[README]]
- [[snowflake]] — the design Databend's product shape echoes.
- [[../data/datafusion/README|DataFusion]] — Rust columnar engine substrate; Databend has its own engine but the ecosystem overlap is heavy.
- [[../../../db/relational/singlestore|SingleStore]] — HTAP comparison.
- [[../../../db/distributed/README|distributed DBs]]

## Keywords

`#databend` `#data-warehouse` `#rust` `#snowflake-alternative` `#clickhouse-alternative` `#udf`

## References / raw notes

- Project home: <https://databend.rs/>
- Source: <https://github.com/datafuselabs/databend>
- Databend Cloud (managed): <https://databend.com/>
- UDF reference: <https://databend.rs/doc/sql-commands/ddl/udf/ddl-create-function>
- Tabular UDF (UDTF) tracking issue: <https://github.com/datafuselabs/databend/issues/5731>

### UDF example (SQL lambda)

```sql
CREATE FUNCTION a_plus_3 AS (a) -> a+3;
SELECT a_plus_3(2);
-- 5

-- JSON-extract style
CREATE FUNCTION get_v1 AS (json) -> json["v1"];
CREATE FUNCTION get_v2 AS (json) -> json["v2"];

CREATE TABLE json_table(time TIMESTAMP, data JSON);
INSERT INTO json_table VALUES('2022-06-01 00:00:00.00000',
                              PARSE_JSON('{"v1":1.5, "v2":20.5}'));

SELECT get_v1(data), get_v2(data) FROM json_table;
-- 1.5  20.5

DROP FUNCTION get_v1;
DROP FUNCTION get_v2;
DROP TABLE json_table;
```

### Connecting

Databend exposes a MySQL-compatible wire protocol — point any MySQL client (`mysql`, `mysqlsh`, `mycli`, the Rust [[../data/mysql|`mysql_async`]] crate, `sqlx` with the `mysql` feature) at the Databend `query` port.
