---
title: DataFusion SQL query planner
main_link: https://github.com/apache/datafusion/tree/main/datafusion/sql
keywords: [datafusion, sql, planner, logical-plan, query-engine, rust, apache]
status: reviewed
---

# DataFusion SQL query planner

**Main link:** <https://github.com/apache/datafusion/tree/main/datafusion/sql>

## Summary

The `datafusion-sql` crate is the **SQL frontend / planner** of [Apache DataFusion](../data/datafusion/README.md): it takes a SQL string, runs it through [[../data/sqlparser|sqlparser-rs]] for lexing/parsing, then translates the resulting AST into DataFusion's logical-plan tree via `SqlToRel`. This crate is intentionally separable from the rest of DataFusion — it makes no assumptions about how the logical plan will be executed (row-vs-columnar, single-node-vs-distributed), so you can reuse it as a SQL planner for your own engine without pulling in the execution side. The downstream `datafusion-optimizer` crate then rewrites the logical plan; only after that do physical planning and execution happen.

## Insight

Reach for `datafusion-sql` directly when you want **just the SQL planning** — for example: building a custom engine that needs ANSI-SQL parsing without inheriting Arrow execution; pre-compiling SQL into stored logical plans for caching; implementing SQL on top of a non-Arrow data source by writing your own `TableProvider` + `SchemaProvider` and converting the resulting `LogicalPlan` yourself. The mental model is a four-stage pipeline: **`sqlparser` AST → `LogicalPlan` (this crate) → optimised `LogicalPlan` (datafusion-optimizer) → `ExecutionPlan` (datafusion-physical-plan)**. Substrait integration lives in the `datafusion-substrait` crate and operates on the optimised logical plan, so if you want cross-engine plan portability that's the layer to look at. The classic gotcha: implementing `ContextProvider` / `SchemaProvider` is more work than it looks because DataFusion expects `Arc<dyn TableProvider>` for every referenced table — the planner is small but its trait surface is wide. For "just parse SQL and inspect the AST" without any planning, drop down to [[../data/sqlparser|`sqlparser-rs`]] directly.

## Similar / related topics

- [[../data/sqlparser|sqlparser-rs]] — the parser this crate sits on top of (Apache fork: `apache/datafusion-sqlparser-rs`).
- [[sql_parse]] — the smaller `sql-parse` crate (MariaDB-focused); cousin not substitute.
- **Apache Calcite** (Java) — the canonical reference for "SQL parser + planner + optimizer" as a reusable library.
- **Substrait** — engine-agnostic logical plan IR; DataFusion can import/export.
- [[sqlite_loadable]] — alternate "extend an engine's SQL surface" path (extension-based, not planner-based).

## Internal links

- [[README]]
- [[../data/datafusion/README]] — DataFusion landing (engine + adopters list).
- [[../data/sqlparser|sqlparser-rs]] — the lexer/parser layer.
- [[roapi]] — concrete consumer of this planner stack.
- [[books]] — Andy Grove's *How Query Engines Work* (the conceptual book behind DataFusion's design).
- [[../../../db/udf/README|db/udf section]]

## Keywords

`#datafusion` `#sql-planner` `#logical-plan` `#query-engine` `#rust` `#apache`

## References / raw notes

- Crate: <https://crates.io/crates/datafusion-sql>
- Source: <https://github.com/apache/datafusion/tree/main/datafusion/sql>
- DataFusion docs: <https://datafusion.apache.org/>

### Minimal example: SQL → LogicalPlan

```rust
use datafusion_sql::sqlparser::dialect::GenericDialect;
use datafusion_sql::sqlparser::parser::Parser;
use datafusion_sql::planner::SqlToRel;

fn main() {
    let sql = "SELECT c.id, c.first_name, c.last_name, \
               COUNT(*) AS num_orders, \
               SUM(o.price) AS total_price \
               FROM customer c \
               JOIN orders o ON c.id = o.customer_id \
               WHERE o.price > 0 \
               GROUP BY 1, 2, 3 \
               ORDER BY total_price DESC";

    let dialect = GenericDialect {};
    let ast = Parser::parse_sql(&dialect, sql).unwrap();

    let schema_provider = MySchemaProvider::new(); // implements ContextProvider
    let sql_to_rel = SqlToRel::new(&schema_provider);
    let plan = sql_to_rel.sql_statement_to_plan(ast[0].clone()).unwrap();

    println!("{plan:?}");
}
```

Unoptimised plan output:

```log
Sort: state_tax DESC NULLS FIRST
  Projection: c.id, c.first_name, c.last_name, COUNT(UInt8(1)) AS num_orders, SUM(o.price) AS total_price, …
    Aggregate: groupBy=[[c.id, c.first_name, c.last_name]], aggr=[[COUNT(UInt8(1)), SUM(o.price), …]]
      Filter: o.price > Int64(0) AND c.last_name LIKE Utf8("G%")
        Inner Join: c.id = o.customer_id
          Inner Join: c.state = s.id
            SubqueryAlias: c
              TableScan: customer
            …
```

Run it through `datafusion-optimizer` to fold predicates / push projections / etc. before handing it to physical planning.
