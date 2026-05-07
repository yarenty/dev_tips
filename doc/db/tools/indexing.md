---
title: SQL indexing (Use The Index, Luke)
main_link: https://use-the-index-luke.com/
keywords: [indexing, sql, b-tree, performance, query-tuning, use-the-index-luke, markus-winand]
status: reviewed
---

# SQL indexing (Use The Index, Luke)

**Main link:** <https://use-the-index-luke.com/>

Book: <https://sql-performance-explained.com/>

## Summary

*Use The Index, Luke* is the canonical free web book on **SQL indexing for
developers** by Markus Winand. It deliberately ignores DBA-flavoured topics
(storage, partitioning, statistics tuning) and focuses on the part developers
actually own: writing queries and DDL such that the database can use indexes
efficiently. It's the web edition of the paid book *SQL Performance
Explained*; both cover Postgres, Oracle, SQL Server, MySQL, and DB2.

## Insight

Why this is the recommended starting point:

- **It's developer-shaped, not DBA-shaped.** Most "performance tuning" books
  drown you in `EXPLAIN` flags and storage parameters. This one starts from
  "your `WHERE` clause looks like this; here's how the index has to look."
- **It treats ORMs honestly.** A whole chapter on Hibernate/JPA/etc. — how
  they generate queries, where the typical anti-patterns come from, how to
  fix them without dropping to native SQL.
- **It's database-agnostic** in the way that matters: the B-tree mental model
  is the same everywhere, and per-engine quirks are called out where they
  matter.

The mental model worth absorbing first:

1. A B-tree index is a sorted list of `(indexed_columns, row_pointer)`.
2. For a multi-column index `(a, b, c)`, the database can efficiently use it
   for `WHERE a = ?`, `WHERE a = ? AND b = ?`, `WHERE a = ? AND b = ? AND c = ?`,
   *and* for `ORDER BY a, b, c` — but **not** for `WHERE b = ?` alone.
3. Anything that hides a column behind a function (`WHERE upper(name) = ?`,
   `WHERE date_trunc('day', ts) = ?`) defeats the index unless you build a
   matching expression index.
4. Range predicates (`>`, `<`, `BETWEEN`) terminate the useful prefix — only
   columns *before* the range in the index key can use equality matching.

If you only ever read one chapter: "The Equality Operator", followed by
"The Like Operator" (leading `%` defeats indexes; trailing `%` is fine).

## Similar / related topics

- **Markus Winand's blog** — <https://winand.at/> — modern SQL features
  (window functions, recursive CTEs, etc.) with the same developer focus.
- **PostgreSQL `EXPLAIN (ANALYZE, BUFFERS)`** — the empirical companion to
  the theory.
- **`pg_stat_statements`** — find the queries that actually need indexing.
- **DuckDB / SQLite query plans** — much simpler optimisers; useful for
  intuition.

## Internal links

- [[db/tools/README|tools]] — section landing.
- [[postgresql]] — the most fully-featured open-source target for what this
  book teaches.
- [[mysql]] — the other primary target.
- [[db/relational/README|relational]] — engines these techniques apply to.

## Keywords

`#indexing` `#sql` `#b-tree` `#performance` `#query-tuning` `#use-the-index-luke` `#postgres` `#mysql`

## References / raw notes

- Free web edition: <https://use-the-index-luke.com/>
- Paid book *SQL Performance Explained*:
  <https://sql-performance-explained.com/>

> SQL indexing is the most effective tuning method — yet it is often
> neglected during development. *Use The Index, Luke* explains SQL indexing
> from the ground up and doesn't stop at ORM tools like Hibernate.
