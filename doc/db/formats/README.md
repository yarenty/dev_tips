---
title: Data formats
keywords: [formats, parquet, arrow, orc, avro, iceberg, delta, hudi, nimble, arrow-flight]
status: reviewed
review_date: 2026/05/03
---

# Data formats

This section covers the *physical* and *logical* formats data sits in: how
bytes are arranged on disk, how a "table" is defined as a collection of files,
and how data moves between systems on the wire.

The vocabulary you actually need:

- **File formats** — how a single file is laid out.
  - Columnar, on-disk: **Parquet**, **ORC**, **Nimble** (Meta).
  - Columnar, in-memory: **Apache Arrow**.
  - Row-oriented: **Avro**, **JSON**, CSV (don't, but you will).
- **Table formats** — how a directory of files becomes an ACID table with
  schema evolution and time-travel.
  - **Apache Iceberg**, **Delta Lake**, **Apache Hudi**.
- **Transfer / wire formats** — how to ship table data between processes
  faster than JDBC/ODBC.
  - **Apache Arrow Flight**, **Arrow Flight SQL**, **ADBC**.

## Articles in this section

- [[db/formats/nimble_file_format/README|Nimble]] — Meta's columnar file format,
  positioned as a successor to ORC.
- [[db/formats/table_transfer_protocols/README|Table transfer protocols]] —
  Arrow Flight, Flight SQL, ADBC, and friends.

## See also

- [[db/catalogs/README|catalogs]] — table formats are usually registered
  inside a catalog.
- [[db/analytics/README|analytics]] — engines that read these formats.
- [[db/distributed/README|distributed]] — sharding and replication strategies
  for files in object stores.

## Keywords

`#formats` `#parquet` `#arrow` `#orc` `#avro` `#iceberg` `#delta` `#hudi` `#nimble` `#arrow-flight`
