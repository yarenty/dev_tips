# SQL UDF 



## Review


This is support of UDFs created using SQL syntax. Generally is something like:

snowflake:
```sql
create function profit()
  returns numeric(11, 2)
  as
  $$
    select sum((retail_price - wholesale_price) * number_sold)
        from purchases
  $$
```

databend:
```sql
CREATE FUNCTION a_plus_3 AS (a) -> a+3;

```
Short table, focusing on rust ecosystem + Spark since our main goal is to check how to quickly and simply move from Java/Spark UDFs to Datafusion UDFs.


| Engine | UDF | UDAF | UDTF | Comment                                                     |  
|----|:---:|:----:|:----:|-------------------------------------------------------------|   
| Spark|  +  |  +   |  -   | _Generally all of UDF/UDAF are excluded from optimizations_ |  
| Hive |  +  |  +   |  +   |                                                             |  
| Snowflake |  +  |  -   |  +   |                                                             |  
| Databend |  +  |  -   |  -   | Databend UDFs are quite primitive                           | 
| Datafusion | + | + | - | Rust API                                                    |
| mariaDB |  +  |  +   |  -   | Big support in Rust ecosystem                               |  
| sqlite |  +  |  +   | -   | Sqlite loadable extensions                                  |  



## Snowflake
- generally quite advanced 
- Supports UDF and UDTF - there are no UDAFs!
- Supports SQL, Java, Javascript, Python
- Only Java and Python are precompiled - SQL is parsed on demand

[more info](snowflake.md)


## mariaDB - sql-udf

MariaDB support in rust is one of the most advanced currently.
There is some libs, but form UDF perspective sql-udf is quite interesting.
It contains both: 
- building UDF/UDAFs using simple interfaces - in Rust
- building .so libraries which are very simple accessible from native mariaDB SQL like:
```sql
CREATE FUNCTION is_const RETURNS string SONAME 'libudf_examples.so';
CREATE FUNCTION lookup6 RETURNS string SONAME 'libudf_examples.so';
CREATE FUNCTION sum_int RETURNS integer SONAME 'libudf_examples.so';
CREATE FUNCTION udf_sequence RETURNS integer SONAME 'libudf_examples.so';
CREATE FUNCTION lipsum RETURNS string SONAME 'libudf_examples.so';
CREATE FUNCTION log_calls RETURNS integer SONAME 'libudf_examples.so';
CREATE AGGREGATE FUNCTION avg2 RETURNS real SONAME 'libudf_examples.so';
CREATE AGGREGATE FUNCTION avg_cost RETURNS real SONAME 'libudf_examples.so';
CREATE AGGREGATE FUNCTION udf_median RETURNS integer SONAME 'libudf_examples.so';
```

[more info](udf_lib.md)


## sqlite

SQLite has a powerful extension mechanism: loadable extensions.

Being an in-process database, SQLite has other extensions mechanisms like application-defined functions (UDF for short). But UDFs have some shortcomings:

[more ](sqlite_loadable.md)


## Rust libraries

### rawsql

https://crates.io/crates/rawsql

A rust library for using and reusing SQL.  - It's like naming queries.
The sql file needs to be with this format :
```sql
-- name: insert-person
INSERT INTO "person" (name, data) VALUES ($1, $2);
```
 from above 'insert-person' query will be created.
[more info](rawsql.md)


### SQL-PARSE

https://crates.io/crates/sql-parse

Parse SQL into an AST.
List of functions - much bigger than in Datafusion.

TODO: Supported dialects  MariaDB, PostgreSQL - how to create own / Datafusion dialect

[more info](sql_parse.md)


### sqlparser

Datafusion is based on this

[more info](sqlparser.md)



## Propositions


### Pre-compiled SQL UDFs

[Datafusion SQL planner](datafusion.md) is based on sqlparser. Datafusion SQL query planner is quite extensible and generally -  ready to be used in sepaae projects.
 



## Questions / Notes

1. There are multiple solutions - while all support UDF - which are scalar ones, UDAFs and UDTFs are more complex and not widely supported. What is current need/ usage of them?
2. Performance - note that we will be comparing with Spark, and Spark UDFs are excluded from optimizations.


## Summary

1. There is possibility to extend sqlparser - even more: Datafusion SQL parser - to have SQL type UDFs.
2. Proposed SQL UDFs could:
    - be compiled on-demand which could be easier that pre-compiled - no need for specific library creation / management / versioning / security 
    - be pre-compiled - into AST - however not really see value over on-demand compilation since whole compilation process is very performant "sub-seconds", and there is possibility of loosing specific optimizations that could be introduced with newest releases / versions of Datafusion;
3. Looking into MariaDB / SQLite development in area of UDFs - they both going into direction of moving into specific Rust APIs - where you can build loadable functions compiled as libraries


## Benefits

- Time-to-market - providing SQL UDF should simplify creation / moving existing Spark UDFs into Datafusion
- With proper structure - could use Datafusion optimization - advance against Spark
- Snowflake / Databend  are supporting scalar UDF, where UDAFs functionality missing
- 

