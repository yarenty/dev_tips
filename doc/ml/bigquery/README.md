---
title: BigQuery & BigQuery ML
keywords: [bigquery, bigquery-ml, sql-ml, google-cloud, warehouse]
status: reviewed
---

# BigQuery & BigQuery ML

A small section about **Google BigQuery** — Google Cloud's serverless,
columnar data warehouse — and specifically **BigQuery ML**, the
in-warehouse ML feature that lets you train and serve models with
plain SQL.

## What is BigQuery ML?

BigQuery ML adds `CREATE MODEL ... OPTIONS(model_type='...') AS
SELECT ...` to BigQuery's SQL dialect. The model is stored as a
first-class BigQuery object alongside your tables; predictions come
back as plain `SELECT` results from `ML.PREDICT(MODEL ..., TABLE ...)`.
Supported model families now include linear/logistic regression,
boosted trees (XGBoost), DNN classifier/regressor, k-means, PCA,
matrix factorisation, ARIMA+ for time-series, AutoML Tables
import/export, and **remote models** that delegate to Vertex AI /
Gemini for embeddings and generation.

This pattern — **SQL as the ML interface** — is now mainstream:
**Snowflake Cortex / Snowflake ML**, **Databricks AI Functions**,
**Postgres + pg_ml / pgvector**, and the substrate-agnostic
[[../frameworks/mindsdb|mindsdb]] are all variations on the same
idea. Pick the one that lives closest to your data.

When BigQuery ML is the right answer:

- Your data already lives in BigQuery; lifting it out for Python ML
  is the most expensive step in the pipeline.
- Your ML problem is well-served by the supported model families
  (it usually is — boosted trees + DNN cover most tabular tasks).
- You want predictions joinable with your existing SQL; the same
  team that writes dashboards can write `ML.PREDICT(...)` queries.

When it isn't:

- You need bespoke architectures, custom losses, or PyTorch-style
  research workflows — escalate to **Vertex AI** or roll your own.
- Your data lives in another warehouse — use that warehouse's
  native SQL-ML (Snowflake Cortex, Databricks AI Functions) or
  [[../frameworks/mindsdb|mindsdb]] for cross-substrate work.

## Articles

- [BigQuery ML — curated reading list](links.md) — the canonical
  Google docs (introduction, `CREATE DNN`, `CREATE TIME_SERIES`)
  plus a hands-on Kaggle tutorial.

## Keywords

`#bigquery` `#bigquery-ml` `#sql-ml` `#google-cloud` `#warehouse`

## See also

- [[../frameworks/mindsdb|mindsdb]] — substrate-agnostic SQL-ML
  layer; the warehouse-independent cousin of BigQuery ML.
- [[../frameworks/README|frameworks]] — broader ML frameworks hub.
- [[../README|ml]] — top-level ML hub.
- [[../../db/README|db]] — databases hub (BigQuery is a serverless
  columnar warehouse first, ML is a feature on top).
- [[../time_series/README|ml/time_series]] — time-series methods
  (BigQuery ML's `ARIMA_PLUS` lives in this problem space).
