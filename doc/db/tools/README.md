---
title: Database tools
keywords: [tools, sql, indexing, dbt, sqlmap, sqlflow, lineage, transformation, security]
status: reviewed
review_date: 2026/05/03
---

# Database tools

Database-adjacent CLIs and reference resources that don't fit cleanly into
any single engine subsection. The four areas covered here:

- **Index design** — knowing what indexes to build and why.
- **SQL-as-code transformation** — dbt-style model trees, version-controlled.
- **Streaming SQL** — SQL on top of Kafka / streams.
- **SQL injection pentest** — auditing your *own* SQL endpoints.

## Articles

- [[indexing]] — *Use The Index, Luke* — the canonical free reference for SQL
  index design.
- [[quary]] — open-source SQL transformation framework (dbt-alternative,
  Rust + DuckDB).
- [[sqlflow]] — streaming SQL on Kafka, built on DuckDB + Arrow.
- [[sqlmap]] — open-source SQL injection / DB takeover pentest tool.
  **Defensive auditing only.**

## See also

- [[db/relational/README|relational]] — engines these tools operate against.
- [[db/streaming/README|streaming]] — broader streaming-SQL context for SQLFlow.
- [[db/quality/README|quality]] — dbt-style tests overlap with quality
  validation.
- [[rustscan]], [[sniffnet]] — adjacent network/security tooling.

## Keywords

`#tools` `#sql` `#indexing` `#dbt` `#sqlmap` `#sqlflow` `#lineage` `#transformation` `#security`
