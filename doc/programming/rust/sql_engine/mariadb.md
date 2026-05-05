---
title: MariaDB
main_link: https://github.com/pluots/sql-udf
keywords: [mariadb, rust, sql, udf, mysql, console]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# MariaDB

**Main link:** <https://github.com/pluots/sql-udf>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[databend]] — Databend _(score 33.5)_
- [[spark]] — Spark UDF _(score 27.7)_
- [[snowflake]] — Snowflake _(score 27.7)_
- [[hive]] — Hive UDF – User Defined Function with Example _(score 23.2)_
- [[udf_lib]] — udf _(score 21.5)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#mariadb` `#sql-engine` `#rust` `#programming` `#sql` `#udf` `#pluots` `#examples`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# MariaDB
https://github.com/pluots/sql-udf

https://github.com/pluots/sql-udf/tree/main/udf-examples


You will need to enter a SQL console to load the functions. This can be done with the mariadb/mysql command, either on your host system or within the docker container. If you used the provided Dockerfile.examples, the password is example.

docker exec -it mariadb_udf_test mysql -pexample
Once logged in, you can load all available example functions:
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
