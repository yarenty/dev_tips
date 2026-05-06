---
title: Data quality (in practice)
main_link: https://github.com/sparkdq-community/sparkdq
keywords: [data-quality, validation, sparkdq, spark, pyspark, great-expectations, soda, lakehouse]
status: reviewed
---

# Data quality (in practice)

**Main link:** <https://github.com/sparkdq-community/sparkdq>

## Summary

"Data quality" is shorthand for systematically catching the cases where data
arrives wrong, drifts wrong, or *was always wrong but you only just noticed*.
The folklore-standard framing splits it across six dimensions —
**completeness, consistency, accuracy, validity, uniqueness, timeliness** —
and the practical work is wiring those checks into ingestion, transformation,
and serving so violations surface early instead of as a downstream BI ticket.
Tools (Great Expectations, Soda, SparkDQ, dbt tests, Pandera, whylogs) help,
but the real win is making quality *part of the pipeline* rather than an
after-the-fact dashboard.

## Insight

A few honest takes:

- **The six dimensions are a checklist, not a method.** They're useful as a
  vocabulary so you know which kind of bug you're chasing (a duplicated row
  is a *uniqueness* failure, a stale partition is a *timeliness* failure).
  But they don't tell you *how* to enforce anything.
- **Quality is a process, not a tool.** Picking Great Expectations vs Soda
  vs SparkDQ is a one-day decision; getting your team to actually maintain
  the expectation suites is the multi-quarter problem.
- **Fail early.** Validate at ingestion (raw → bronze) and *before* writing
  to a curated table (silver → gold). Validating after the fact in a separate
  job is much weaker — the bad data has already shipped.
- **Severity matters.** A good framework distinguishes "warn" from "block".
  Without that, every check becomes either ignored or a P0.
- **Data contracts** are the producer-side mirror of validation: the upstream
  team commits to a schema + invariants, and the consumer runs the same
  checks. Treat them as a coordination tool, not a vendor product.
- **For ML data**, the standard six dimensions miss the point — see
  [[db/quality/data_quality_problems_in_AI/README|data quality in AI]] for
  label noise, distribution shift, prompt-data interaction.

### Where SparkDQ fits

SparkDQ is a **PySpark-native** validation framework — checks are first-class
Spark operations, not a wrapper that pulls data into pandas. It's worth
knowing about because most general-purpose tools (Great Expectations, Soda)
still feel like they're bolted onto Spark rather than written for it. SparkDQ:

- Defines checks declaratively (config) or programmatically (Python).
- Distinguishes warning vs critical violations.
- Supports row-level *and* aggregate-level constraints.
- Has zero heavy dependencies — pure PySpark.

Good fit for: lakehouse pipelines (Delta / Iceberg / Hudi) where you want
checks to run in the same Spark job that produced the data, no extra cluster.

## Similar / related topics

- **Great Expectations** — the most widely deployed validation framework,
  Python-first, lots of integrations.
- **Soda Core** — YAML-defined checks, good multi-engine coverage.
- **dbt tests** — the path of least resistance if you already use dbt.
- **Pandera** — typed schema validation for pandas / polars / PySpark.
- **whylogs** — profiling and drift detection; complementary to validation.
- **OpenLineage** — capture lineage so you can trace a quality failure back
  to its source.

## Internal links

<!-- reviewed -->

- [[db/quality/README|quality]] — section landing.
- [[db/quality/data_curation/README|data curation]] — the active-curation
  cousin of validation.
- [[db/quality/data_quality_problems_in_AI/README|data quality in AI]] — the
  ML-specific failure modes.
- [[db/catalogs/README|catalogs]] — where lineage and quality results
  typically live.
- [[db/streaming/README|streaming]] — quality enforcement on streams is its
  own art.

## Keywords

`#data-quality` `#quality` `#sparkdq` `#spark` `#pyspark` `#validation` `#great-expectations` `#soda` `#lakehouse`

## References / raw notes

### SparkDQ — Data Quality Validation for Apache Spark

Repo: <https://github.com/sparkdq-community/sparkdq>

Most data quality frameworks weren't designed with PySpark in mind. They
aren't Spark-native and often lack proper support for declarative pipelines.
Instead of integrating seamlessly, they require you to build custom wrappers
around them just to fit into production workflows. This adds complexity and
makes pipelines harder to maintain. On top of that, many frameworks only
validate data *after* processing — so you can't react dynamically or fail
early when data issues occur.

SparkDQ takes a different approach. It's built specifically for PySpark — so
you can define and run data quality checks directly inside your Spark
pipelines, using Python. Whether you're validating incoming data, verifying
outputs before persistence, or enforcing assumptions in your dataflow,
SparkDQ helps catch issues early without adding complexity.

#### Why SparkDQ?

- Robust validation layer: clean separation of check definition, execution,
  and reporting.
- Declarative or programmatic: define checks via config files or directly in
  Python.
- Severity-aware: built-in distinction between warning and critical violations.
- Row & aggregate logic: supports both record-level and dataset-wide
  constraints.
- Typed & tested: built with type safety, testability, and extensibility in
  mind.
- Zero overhead: pure PySpark, no heavy dependencies.

#### Typical use cases

- Validating raw ingestion data.
- Enforcing schema and content rules before persisting to a lakehouse
  (Delta, Iceberg, Hudi).
- Asserting quality conditions before analytics or ML training jobs.
- Flagging critical violations in batch pipelines via structured summaries
  and alerts.
- Driving data contracts: declarative checks in CI to catch issues before
  deployment.
