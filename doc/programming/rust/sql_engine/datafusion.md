---
title: Datafusion SQL Query Planner
main_link: https://github.com/apache/arrow-datafusion/tree/master/datafusion/sql
keywords: [rust, sql, planner, query, apache]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Datafusion SQL Query Planner

**Main link:** <https://github.com/apache/arrow-datafusion/tree/master/datafusion/sql>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[roapi]] — ROAPI _(score 26.6)_
- [[programming/rust/sql_engine/sqlparser|sqlparser]] — SQLparser _(score 21.5)_
- [[db]] — diesel _(score 17.5)_
- [[programming/rust/data/sqlparser|sqlparser]] — sqlparser _(score 17.5)_
- [[qpml]] — QPML _(score 17.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#datafusion` `#sql-engine` `#rust` `#programming` `#sql` `#query` `#planner` `#logical`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Datafusion SQL Query Planner

While what we looking for is to extend DF with possiblility to have SQL type UDFs - here I will quickly get what is there.

Note: DF SQL parser is based on [sqlparser](sqlparser.md).

And if we would like to have "pre-compiled" SQLs - this is part which we would probably extend!

@see:

https://github.com/apache/arrow-datafusion/tree/master/datafusion/sql






## DataFusion SQL Query Planner
This crate provides a general purpose SQL query planner that can parse SQL and translate queries into logical plans. Although this crate is used by the DataFusion query engine, it was designed to be easily usable from any project that requires a SQL query planner and does not make any assumptions about how the resulting logical plan will be translated to a physical plan. For example, there is no concept of row-based versus columnar execution in the logical plan.

## Example Usage
See the examples directory for fully working examples.

Here is an example of producing a logical plan from a SQL string.
```rust
fn main() {
    let sql = "SELECT \
            c.id, c.first_name, c.last_name, \
            COUNT(*) as num_orders, \
            SUM(o.price) AS total_price, \
            SUM(o.price * s.sales_tax) AS state_tax \
        FROM customer c \
        JOIN state s ON c.state = s.id \
        JOIN orders o ON c.id = o.customer_id \
        WHERE o.price > 0 \
        AND c.last_name LIKE 'G%' \
        GROUP BY 1, 2, 3 \
        ORDER BY state_tax DESC";

    // parse the SQL
    let dialect = GenericDialect {}; // or AnsiDialect, or your own dialect ...
    let ast = Parser::parse_sql(&dialect, sql).unwrap();
    let statement = &ast[0];

    // create a logical query plan
    let schema_provider = MySchemaProvider::new();
    let sql_to_rel = SqlToRel::new(&schema_provider);
    let plan = sql_to_rel.sql_statement_to_plan(statement.clone()).unwrap();

    // show the plan
    println!("{:?}", plan);
}
```


This is the logical plan that is produced from this example. Note that this is an unoptimized logical plan. The datafusion-optimizer crate provides a query optimizer that can be applied to plans produced by this crate.


```log
Sort: state_tax DESC NULLS FIRST
  Projection: c.id, c.first_name, c.last_name, COUNT(UInt8(1)) AS num_orders, SUM(o.price) AS total_price, SUM(o.price * s.sales_tax) AS state_tax
    Aggregate: groupBy=[[c.id, c.first_name, c.last_name]], aggr=[[COUNT(UInt8(1)), SUM(o.price), SUM(o.price * s.sales_tax)]]
      Filter: o.price > Int64(0) AND c.last_name LIKE Utf8("G%")
        Inner Join: c.id = o.customer_id
          Inner Join: c.state = s.id
            SubqueryAlias: c
              TableScan: customer
            SubqueryAlias: s
              TableScan: state
          SubqueryAlias: o
            TableScan: orders

```
