---
title: Table transfer protocols
main_link: https://arrow.apache.org/docs/format/Flight.html
keywords: [arrow-flight, flight-sql, adbc, jdbc, odbc, transfer, protocol, arrow]
status: reviewed
---

# Table transfer protocols

**Main link:** <https://arrow.apache.org/docs/format/Flight.html>

Arrow Flight: <https://arrow.apache.org/docs/format/Flight.html> ·
Flight SQL: <https://arrow.apache.org/docs/format/FlightSql.html> ·
ADBC: <https://arrow.apache.org/docs/format/ADBC.html>

## Summary

A "table transfer protocol" is the wire-format question: when a client wants
rows back from a database, how do those rows actually get serialised and
streamed? The legacy answers are **JDBC** and **ODBC**, both row-oriented and
both designed in an era when networks were the bottleneck. Modern columnar
engines move data in much larger, much wider batches than that, and serialising
to row tuples just to deserialise into Arrow on the other side is wasteful.
The modern answers — **Apache Arrow Flight**, **Flight SQL**, and **ADBC** —
ship Arrow batches over gRPC end to end.

## Insight

When to care:

- **You're moving large result sets** — millions of rows from an analytics
  warehouse to a Python/Spark client. Flight can be 10–100× faster than ODBC
  in those benchmarks because it avoids per-row serialisation and parallelises
  fetch streams natively.
- **Your engine and your client both speak Arrow** — DuckDB, Dremio, Snowflake
  ADBC driver, ClickHouse Arrow output, Spark Arrow exchange, BigQuery Storage
  Read API (similar idea). At that point, JDBC/ODBC is just a row-shaped
  conversion tax.
- **You want a "BigQuery Storage Read API but vendor-neutral"** — that's
  basically Flight SQL.

The three names sort like:

- **Arrow Flight** — generic gRPC + Arrow streaming protocol. Not SQL-specific.
- **Arrow Flight SQL** — Flight + a SQL-database-shaped command set
  (`Execute`, `GetTables`, `GetSchemas`, prepared statements). The direct
  JDBC/ODBC analogue, but Arrow-native.
- **ADBC** — *API* (not wire protocol) — a JDBC/ODBC-style C/Java/Python
  client API whose drivers can be backed by Flight SQL, native protocols, or
  even ODBC adapters. This is what application code should target.

When *not* to care: small OLTP queries (a few rows), CRUD apps, classical web
backends. JDBC/ODBC are fine and the tooling is overwhelming.

## Similar / related topics

- **JDBC / ODBC** — the row-oriented incumbents.
- **BigQuery Storage Read API** — Google's proprietary equivalent.
- **Snowflake Arrow result format** — vendor-specific Arrow over HTTPS.
- **Apache Arrow** — the in-memory columnar format these protocols ship.
- **gRPC** — the transport Flight rides on.

## Internal links

- [[db/formats/README|formats]] — section landing.
- [[db/formats/nimble_file_format/README|Nimble]] — file-format counterpart.
- [[db/analytics/README|analytics]] — primary consumers of Flight SQL.
- [[db/distributed/README|distributed]] — parallel fetch streams matter most
  for distributed engines.

## Keywords

`#arrow-flight` `#flight-sql` `#adbc` `#jdbc` `#odbc` `#arrow` `#grpc` `#transfer-protocol`

## References / raw notes

- Arrow Flight: <https://arrow.apache.org/docs/format/Flight.html>
- Arrow Flight SQL: <https://arrow.apache.org/docs/format/FlightSql.html>
- ADBC: <https://arrow.apache.org/docs/format/ADBC.html>
