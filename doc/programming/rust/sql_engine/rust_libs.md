---
title: SQL Server (T-SQL) — UDF angle, Rust pointers
main_link: https://learn.microsoft.com/en-us/sql/relational-databases/user-defined-functions/create-user-defined-functions-database-engine
keywords: [sql-server, mssql, t-sql, udf, tiberius, clr]
status: reviewed
review_date: 2026/05/03
---

# SQL Server (T-SQL) — UDF angle, Rust pointers

**Main link:** <https://learn.microsoft.com/en-us/sql/relational-databases/user-defined-functions/create-user-defined-functions-database-engine>

## Summary

Microsoft SQL Server supports user-defined functions natively as **T-SQL UDFs** (`CREATE FUNCTION … AS BEGIN … END`) in both scalar and table-valued (TVF: inline or multi-statement) flavours. For non-T-SQL logic the path is **CLR integration** — write a .NET assembly (C# / VB.NET / F#), `CREATE ASSEMBLY` it into the database, then `CREATE FUNCTION … EXTERNAL NAME …`. There is **no native Rust UDF SDK for SQL Server**; if you want to call into Rust you'd ship a Rust `cdylib` and wrap it with a thin C# CLR assembly that P/Invokes into it. For the Rust *client* side (talking to SQL Server from a Rust app), see [[../data/tiberius|Tiberius]] — that's the canonical and only realistic option.

## Insight

Filed in this section for completeness — the "Rust SQL-engine" content here is mostly via the client crate. T-SQL scalar UDFs in SQL Server have a long-standing performance reputation problem (row-by-row execution, opacity to the optimizer); SQL Server 2019+ introduced **Scalar UDF Inlining** to mitigate, and inline TVFs have always been parameterized-view-shaped and well-optimised. CLR UDFs are out of fashion (the surface for Rust integration is small and fiddly: SQL Server runs CLR in a sandboxed AppDomain, P/Invoking into a Rust `cdylib` is possible but requires `EXTERNAL_ACCESS`/`UNSAFE` permission set and signed assemblies). Modern alternatives if your goal is "push compute into the DB": for OLAP, Microsoft Fabric / Synapse spark pools (Scala/Python); for OLTP, the SQL Server 2022 Azure Arc / Linked Server angle; for Rust-y workloads, drop the SQL Server requirement and use Postgres + `pgrx` instead.

## Similar / related topics

- [[../data/tiberius|Tiberius]] — pure-Rust async TDS client; the canonical Rust-side story.
- **`pgrx`** — Postgres extensions in Rust; the realistic alternative if SQL-Server-specific lock-in isn't a hard constraint.
- [[udf_lib|`udf` (MariaDB/MySQL)]] — by-comparison the most polished Rust UDF story across engines.
- **CLR UDFs (.NET)** — the only Microsoft-blessed non-T-SQL UDF path on SQL Server.
- **SQL Server Machine Learning Services** — `sp_execute_external_script` for Python / R UDF-shaped workloads; not Rust.

## Internal links

- [[README]]
- [[../data/tiberius|Tiberius — Rust async TDS client]]
- [[udf_lib]] — the Rust-UDF reference point.
- [[../../../db/relational/README|relational DBs]]
- [[../../../db/udf/external_udfs|external UDFs survey]]

## Keywords

`#sql-server` `#mssql` `#t-sql` `#udf` `#clr` `#tiberius`

## References / raw notes

- T-SQL `CREATE FUNCTION` (the original link): <https://learn.microsoft.com/en-us/sql/relational-databases/user-defined-functions/create-user-defined-functions-database-engine?view=sql-server-ver16>
- CLR integration: <https://learn.microsoft.com/en-us/sql/relational-databases/clr-integration/clr-integration-overview>
- Scalar UDF inlining (2019+): <https://learn.microsoft.com/en-us/sql/relational-databases/user-defined-functions/scalar-udf-inlining>

> Historical note: this file was originally a multi-topic dump for "Rust libs in the SQL space" with sections for CrateDB, libsql/WASM, and Excel UDFs. Those sections were split out into [[cratedb]], [[wasm_for_libsql]] and [[excel_udf_in_rust]]; this file now focuses on the SQL-Server / T-SQL angle implicit in its title ("MS Server").
