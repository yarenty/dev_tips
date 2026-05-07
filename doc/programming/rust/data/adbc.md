---
title: ADBC — Arrow Database Connectivity
main_link: https://arrow.apache.org/adbc/
keywords: [adbc, arrow, database, jdbc, odbc, columnar, flight-sql, rust]
status: reviewed
---

# ADBC — Arrow Database Connectivity

**Main link:** <https://arrow.apache.org/adbc/>

## Summary

ADBC (Arrow Database Connectivity) is an Apache-Arrow-curated API standard for database access libraries (drivers) in C, Go, Java, Python, and Rust that uses Arrow record batches as the wire format for both result sets and bound parameters. Like JDBC/ODBC it is a generic client-side abstraction with a driver-manager that dynamically loads per-database drivers, but unlike JDBC/ODBC it is built specifically for **bulk columnar data movement** — no per-row marshalling, no Java/C struct conversions. The Rust implementation lives in [`arrow-adbc/rust`](https://github.com/apache/arrow-adbc/tree/main/rust).

## Insight

Reach for ADBC when the bottleneck of an existing JDBC/ODBC pipe is the row-by-row marshalling cost (typical when piping a Postgres/Snowflake/DuckDB result set into a Polars/pandas/Arrow-native consumer). The mental model: ODBC was built when "row" was the unit of transfer; ADBC is built when "RecordBatch" is. ADBC and Apache Arrow Flight SQL are complementary — Flight SQL defines a **wire protocol** (gRPC) that databases implement on the server side, while ADBC defines a **client API** that wraps any underlying protocol (Flight SQL, native PG, native MySQL, ...). Together they form an end-to-end Arrow-native stack. Gotchas: the Rust crate is still pre-1.0 and the driver coverage is thinner than JDBC's (the canonical drivers as of 2024 are PostgreSQL, SQLite, Snowflake, DuckDB, BigQuery, Flight SQL).

## Similar / related topics

- Arrow Flight SQL — Arrow-native server-side wire protocol; ADBC drivers can wrap it.
- JDBC / ODBC — the row-oriented predecessors; ADBC is explicitly *complementary*, not a replacement for transactional workloads.
- `tokio-postgres` / `sqlx` — row-oriented Rust drivers; switch when you need Arrow output.
- DuckDB / DataFusion — both ship native Arrow result sets; ADBC is the standard way to consume them from a generic client.

## Internal links
<!-- reviewed -->
- [[db/formats/table_transfer_protocols/README|Table transfer protocols (Arrow Flight / ADBC)]]
- [[datafusion/README|DataFusion family]]
- [[lance_data_format]] — Arrow-friendly columnar format
- [[programming/rust/io/parsers|Rust parsers]] — adjacent column-oriented IO

## Keywords

`#adbc` `#arrow` `#columnar` `#jdbc` `#odbc` `#flight-sql`

## References / raw notes

- Project site: <https://arrow.apache.org/adbc/>
- Repo: <https://github.com/apache/arrow-adbc/tree/main>
- Rust crate: <https://github.com/apache/arrow-adbc/blob/main/rust/src/lib.rs>
- Introductory blog post: [Introducing ADBC: Database Access for Apache Arrow](https://arrow.apache.org/blog/2023/01/05/introducing-arrow-adbc/)

ADBC versions the API standard and the implementing libraries separately; the API standard (1.0.0) is considered stable, while individual driver crates are pre-1.0 and under active development.
