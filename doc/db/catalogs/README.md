---
title: Data catalogs
keywords: [catalogs, metadata, governance, lineage, hive-metastore, unity-catalog, glue, iceberg]
status: reviewed
---

# Data catalogs

A *data catalog* is the metadata layer over many storage engines: it's where
"what tables exist, what columns, what owns them, what lineage produced them,
who is allowed to read them" lives — independent of the actual SQL engine that
runs queries. Catalogs are how a Spark job, a Trino cluster, a notebook, and a
BI tool can all see the same physical Parquet files as the same *table*.

## Articles in this section

- [[db/catalogs/unity_hive_catalog/README|Unity Catalog]] — Databricks' catalog,
  the modern successor to the Hive Metastore.

## External alternatives worth knowing

- **Hive Metastore** — the original, still everywhere as a compatibility shim.
- **AWS Glue Data Catalog** — Hive-Metastore-compatible managed service on AWS.
- **Apache Atlas** — open-source governance / lineage from the Hadoop world.
- **OpenMetadata** — modern open catalog with active community, lineage, quality.
- **Apache Polaris** — open-spec REST catalog (Snowflake-originated) for Iceberg.
- **Apache Iceberg REST catalog spec** — the protocol Polaris and others speak;
  the most likely "open standard" answer if you don't want vendor lock-in.

## See also

- [[db/formats/README|formats]] — table formats (Iceberg, Delta, Hudi) that
  catalogs index.
- [[db/quality/README|quality]] — quality checks usually live alongside the
  catalog (lineage + tests).
- [[db/fabric/README|fabric]] — "data fabric" is largely catalog + integration
  + governance under a marketing umbrella.

## Keywords

`#catalogs` `#metadata` `#governance` `#lineage` `#unity-catalog` `#hive-metastore` `#glue` `#iceberg` `#polaris`
