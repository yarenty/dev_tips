---
title: Snowflake — UDFs, Snowpark, and the Rust client story
main_link: https://docs.snowflake.com/en/sql-reference/udf-overview
keywords: [snowflake, udf, sql, snowpark, data-warehouse, rust-client]
status: reviewed
---

# Snowflake — UDFs, Snowpark, and the Rust client story

**Main link:** <https://docs.snowflake.com/en/sql-reference/udf-overview>

## Summary

Snowflake is the canonical commercial cloud data warehouse with separated storage (object store) + compute (virtual warehouses), columnar SQL engine, and a polished managed offering. This article focuses on **(a) the UDF model** (SQL / JavaScript / Java / Python; scalar and tabular; Snowflake does *not* support user-defined aggregates) and **(b) talking to Snowflake from Rust** (no first-party Rust SDK; the realistic options are ADBC's Snowflake driver and the community `snowflake-api` crate, both Arrow-native).

## Insight

Snowflake's UDF surface is wider than most warehouses but narrower than Postgres + `pgrx`. The four supported languages each have different deployment models — **SQL** UDFs are inline expressions (no body, just a single `SELECT`); **JavaScript** runs inline in a sandbox; **Java** UDFs are *pre-compiled* JARs uploaded to a stage; **Python** UDFs are *staged* source code (so `requirements.txt`-style deps work via Snowpark, with the rule that any package must be platform-independent — no native extensions). **No UDAFs** is the biggest single asymmetry vs MariaDB/Postgres/Spark; the workaround is a tabular UDF over a window or an "approximation" function. **Java UDFs are not data-shareable** via Snowflake's data-sharing feature (sharing a view that calls one fails); same for Python UDFs. From the Rust side, the practical paths in 2024 are: **ADBC** (`adbc_driver_snowflake`, Arrow-native, supported by Snowflake) — see [[../data/adbc|ADBC]]; the `snowflake-api` community crate (REST-based, smaller footprint, less polished); or **Arrow Flight SQL** if and when Snowflake exposes a Flight endpoint (not yet, as of 2024). Internal-rumours angle: Snowflake has confirmed using Rust for some internal services and has been visibly hiring Rust talent; their Polaris (Iceberg catalogue) implementation is Java but has Rust client work in the broader ecosystem.

## Similar / related topics

- [[databend]] — the Rust open-source warehouse most often pitched as a "Snowflake alternative".
- **BigQuery** — Google's column-store warehouse equivalent; richer ML integration, less SQL-purist.
- **Redshift / Redshift Serverless** — the AWS equivalent.
- **DuckDB** — single-process columnar SQL; the embedded-friendly alternative for non-cloud workloads.
- [[../data/adbc|ADBC]] — Arrow-native database connectivity; the modern way to talk to Snowflake from Rust.

## Internal links

<!-- reviewed -->
- [[README]]
- [[databend]] — Rust open-source equivalent.
- [[../data/adbc|ADBC]] — modern Arrow-native client path.
- [[../../../db/udf/external_udfs|external UDFs survey]]

## Keywords

`#snowflake` `#udf` `#sql` `#snowpark` `#data-warehouse` `#rust-client`

## References / raw notes

- UDF overview: <https://docs.snowflake.com/en/sql-reference/udf-overview.html>
- SQL UDF reference: <https://docs.snowflake.com/en/developer-guide/udf/sql/udf-sql.html>
- Tabular UDF reference: <https://docs.snowflake.com/en/developer-guide/udf/sql/udf-sql-tabular-functions.html>
- Window functions: <https://docs.snowflake.com/en/sql-reference/functions-analytic.html>
- ADBC Snowflake driver: <https://arrow.apache.org/adbc/main/driver/snowflake.html>
- Community Rust crate `snowflake-api`: <https://crates.io/crates/snowflake-api>

### Snowflake UDF language matrix

| Language   | Tabular | Branching/looping | Pre-compiled / staged source | Inline | Sharable |
|------------|---------|-------------------|------------------------------|--------|----------|
| SQL        | Yes     | No                | No                           | Yes    | Yes      |
| JavaScript | Yes     | Yes               | No                           | Yes    | Yes      |
| Java       | Yes     | Yes               | Yes (pre-compiled)           | Yes    | **No**   |
| Python     | Yes     | Yes               | Yes (staged)                 | Yes    | **No**   |

Notes:

- **Java UDFs are not sharable.** You cannot directly share a Java UDF, share a view that calls one, share a function that calls one, or share a table whose masking/row-access policy calls one.
- **Python UDFs are not sharable** (same restrictions as Java) and modules brought in via stages must be platform-independent — no native extensions, no x86-only / OS-only code.
- **No UDAFs**: Snowflake does not support user-defined aggregate functions in any language.

### SQL UDF examples

Inline scalar:

```sql
CREATE FUNCTION add_pi(PARAM_1 FLOAT)
    RETURNS FLOAT
    LANGUAGE SQL
    AS $$
        PARAM_1 + 3.1415926::FLOAT
    $$;
```

Hard-coded constant:

```sql
create function pi_udf()
returns float
as '3.141592654::FLOAT';

select pi_udf();
-- 3.141592654
```

UDF with a query body (must return at most one row, one column for non-tabular):

```sql
create function profit()
returns numeric(11, 2)
as
$$
select sum((retail_price - wholesale_price) * number_sold)
from purchases
$$;
```

UDF in a `WITH` clause:

```sql
create function diameter_to_radius(f float)
returns float
as $$ f / 2 $$;

with radii as (select diameter_to_radius(diameter) as radius from circles)
select radius from radii order by radius;
```

JOIN-bearing UDF:

```sql
create function store_profit()
returns numeric(11, 2)
as
$$
select sum( (o.price - i.price) * o.quantity)
from orders as o, inventory as i
where o.product_id = i.product_id
$$;
```

Using SQL session variables in a UDF:

```sql
set id_threshold = (select count(*)/2 from table1);

create or replace function my_filter_function()
returns table (id int)
as
$$
select id from table1 where id > $id_threshold
$$;
```

### Tabular UDF

```sql
CREATE OR REPLACE FUNCTION <name> ( [ <arguments> ] )
  RETURNS TABLE ( <output_col_name> <output_col_type> [, …] )
  AS '<sql_expression>';

create function t()
    returns table(msg varchar)
    as
    $$
        select 'Hello'
        union
        select 'World'
    $$;
```

### Restrictions on SQL UDF bodies

- The owner must have appropriate privileges on referenced objects.
- Database objects cannot be referred to via dynamic SQL (`IDENTIFIER(table_name_parameter)` is rejected).
- The defining expression must not include a trailing `;` as a statement terminator.
- A UDF can call other UDFs but not recursively, directly or indirectly.
- The body can contain a complete `SELECT` but **no DDL or non-`SELECT` DML**.
