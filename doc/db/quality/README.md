---
title: Data quality
keywords: [data-quality, validation, lineage, profiling, great-expectations, soda, openlineage, ml]
status: reviewed
---

# Data quality

Data quality is the umbrella for "do I trust this dataset?" — and the honest
answer is usually a process, not a tool. This section collects validation
frameworks, lineage / observability for data, profiling, and the variant of
the problem that shows up in ML training.

## Articles in this section

- [[data_quality]] — the substantive overview, including the standard six
  dimensions (completeness, consistency, accuracy, validity, uniqueness,
  timeliness) and where SparkDQ fits.

## Subsections

- [[db/quality/data_curation/README|data curation]] — the deeper, AI-shaped
  variant where you actively select, synthesise, or annotate data for training.
- [[db/quality/data_quality_problems_in_AI/README|data quality in AI]] —
  problems specific to ML: label noise, distribution shift, prompt-data
  interaction in LLMs.

## External tooling worth knowing

- **Validation**: Great Expectations, Soda Core, **SparkDQ** (PySpark-native),
  dbt tests, **Pandera** (typed pandas), **Pointblank**.
- **Profiling / anomaly detection**: **whylogs**, **ydata-profiling**,
  **Anomalo**, **Monte Carlo** (commercial).
- **Lineage / observability**: **OpenLineage**, **Marquez**, **DataHub**,
  **OpenMetadata**.
- **Data contracts**: a discipline more than a tool — see Schemata, dbt
  contracts, Great Expectations data contracts.

## See also

- [[db/catalogs/README|catalogs]] — quality and lineage usually share the
  catalog as their backing store.
- [[db/streaming/README|streaming]] — quality at the stream-processing layer.
- [[db/udf/README|UDF]] — custom validation logic often ships as a UDF.

## Keywords

`#quality` `#data-quality` `#validation` `#lineage` `#profiling` `#great-expectations` `#soda` `#openlineage` `#ml`
