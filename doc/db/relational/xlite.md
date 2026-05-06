---
title: "XLite — query Excel / ODS spreadsheets as SQLite virtual tables"
main_link: https://github.com/x2bool/xlite
keywords: [xlite, sqlite, virtual-table, excel, xlsx, ods, spreadsheet, etl]
status: reviewed
---

# XLite — query Excel / ODS spreadsheets as SQLite virtual tables

**Main link:** <https://github.com/x2bool/xlite>

SQLite virtual tables: <https://www.sqlite.org/vtab.html>

## Summary

XLite is a SQLite extension (loadable module) that exposes Excel `.xlsx` / `.xls` workbooks and OpenDocument `.ods` spreadsheets as SQLite **virtual tables** — meaning you can `SELECT` over them with full SQL: joins, `WHERE`, `GROUP BY`, the works. Written in Rust. It's a perfect example of the SQLite virtual-table mechanism: instead of importing a spreadsheet into a database, you point the DB at the file and query it directly.

## Insight

When this is genuinely useful:

- **Ad-hoc analysis of spreadsheets that are too big or too gnarly for Excel itself.** A few hundred MB of XLSX with twelve sheets and merged cells is much more tractable as SQL.
- **Quick ETL-without-ETL.** Join a spreadsheet against a real SQLite table to enrich or validate.
- **Reproducible reporting.** A small `.sql` script that loads an XLSX and emits a CSV is much easier to re-run than a manual Excel pivot.
- **Light-touch data cleanup** as a step in a larger pipeline before importing into [[postgresql]].

Limitations:

- It's read-oriented. For heavy modification, just convert to a real table.
- For genuine analytics, [DuckDB](https://duckdb.org/) has even richer spreadsheet support (`read_xlsx`, `read_csv_auto`, native Parquet, joins across all of them) and is usually the better choice today.
- Loadable extensions need to be allowed by your SQLite build (some packagers disable them).

If your problem is "I have spreadsheets and I want SQL", XLite is the SQLite-native answer; DuckDB is the modern analytical-engine answer; Python + pandas/polars is the scripted answer.

## References / raw notes

> Query Excel (.xlsx, .xls) and Open Document spreadsheets (.ods) as SQLite virtual tables.

Repo: <https://github.com/x2bool/xlite>

## Similar / related topics

- [SQLite virtual tables](https://www.sqlite.org/vtab.html) — the underlying mechanism.
- [DuckDB](https://duckdb.org/) — broader analytical engine that natively reads spreadsheets, CSV, Parquet, JSON.
- [[limbo]] — Rust SQLite-compatible rewrite (no virtual-table extensions yet).
- [Polars](https://pola.rs/) — DataFrame-shaped alternative for the same job.
- `sqlite-utils` — Simon Willison's CLI for SQLite, very handy alongside XLite.

## Internal links

<!-- reviewed -->

- [[limbo]]
- [[postgresql]]

## Keywords

`#xlite` `#sqlite` `#virtual-table` `#excel` `#xlsx` `#ods` `#spreadsheet` `#etl` `#db`
