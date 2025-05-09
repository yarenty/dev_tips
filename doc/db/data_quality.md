# Data Quality

## SparkDQ


https://github.com/sparkdq-community/sparkdq?utm_source=tldrdata


### SparkDQ — Data Quality Validation for Apache Spark
Most data quality frameworks weren’t designed with PySpark in mind. They aren’t Spark-native and often lack proper support for declarative pipelines. Instead of integrating seamlessly, they require you to build custom wrappers around them just to fit into production workflows. This adds complexity and makes your pipelines harder to maintain. On top of that, many frameworks only validate data after processing — so you can’t react dynamically or fail early when data issues occur.

SparkDQ takes a different approach. It’s built specifically for PySpark — so you can define and run data quality checks directly inside your Spark pipelines, using Python. Whether you're validating incoming data, verifying outputs before persistence, or enforcing assumptions in your dataflow: SparkDQ helps you catch issues early, without adding complexity.

### Why SparkDQ?
✅ Robust Validation Layer: Clean separation of check definition, execution, and reporting

✅ Declarative or Programmatic: Define checks via config files or directly in Python

✅ Severity-Aware: Built-in distinction between warning and critical violations

✅ Row & Aggregate Logic: Supports both record-level and dataset-wide constraints

✅ Typed & Tested: Built with type safety, testability, and extensibility in mind

✅ Zero Overhead: Pure PySpark, no heavy dependencies

### Typical Use Cases
SparkDQ is built for modern data platforms that demand trust, transparency, and resilience. It helps teams enforce quality standards early and consistently — across ingestion, transformation, and delivery layers.

Whether you're building a real-time ingestion pipeline or curating a data product for thousands of downstream users, SparkDQ lets you define and execute checks that are precise, scalable, and easy to maintain.

Common Scenarios:

✅ Validating raw ingestion data

✅ Enforcing schema and content rules before persisting to a lakehouse (Delta, Iceberg, Hudi)

✅ Asserting quality conditions before analytics or ML training jobs

✅ Flagging critical violations in batch pipelines via structured summaries and alerts

✅ Driving Data Contracts: Use declarative checks in CI pipelines to catch issues before deployment