---
title: BigQuery ML — curated reading list
main_link: https://cloud.google.com/bigquery-ml/docs/introduction
keywords: [bigquery, bigquery-ml, sql-ml, dnn, arima, time-series, links]
status: reviewed
---

# BigQuery ML — curated reading list

**Main link:** <https://cloud.google.com/bigquery-ml/docs/introduction>

## Summary

A small curated reading list for **BigQuery ML** — Google Cloud's
in-warehouse ML offering that lets you train and serve models with
plain SQL (`CREATE MODEL ... OPTIONS(model_type=...) AS SELECT ...`).
The canonical Google docs are the centrepiece; the entries below
are the parts of the docs worth bookmarking, plus a community
tutorial for hands-on practice.

## Insight

BigQuery ML is the first place to look if your data already lives
in BigQuery — there's no data movement, no separate ML cluster, and
the cost model is "you pay for the SQL". The supported model
families have grown well past the original linear-and-logistic
regressions: deep neural nets (DNN classifier/regressor), boosted
trees (XGBoost), matrix factorisation, k-means, PCA, ARIMA+ for
time-series, AutoML Tables import/export, and direct calls into
Vertex AI / Gemini for embeddings and generation.

When to read these links:

- You're scoping whether to do an ML task in SQL vs lifting the
  data out into Python — start with the **introduction** and the
  **model-type matrix** in the Google docs.
- You're picking between a hand-tuned DNN and `BOOSTED_TREE_*` —
  the **CREATE DNN** reference page has the syntax + hyperparameter
  options.
- You're fitting time series — the **CREATE_TIME_SERIES** page
  documents `ARIMA_PLUS` and the large-scale variant for
  many-series-at-once.
- You want a worked end-to-end walkthrough — Rachael Tatman's
  Kaggle notebook is one of the better "type-along" tutorials.

Honest framing: BigQuery ML is great for *getting predictions out
of warehouse data without leaving SQL*; it is not the right tool for
deep learning research, custom architectures, or anything that needs
GPU training inside your own VPC. For that, export to Vertex AI or
roll your own pipeline. See [[../frameworks/mindsdb|mindsdb]] for
the substrate-agnostic SQL-ML cousin.

## Similar / related topics

- Snowflake Cortex / Snowflake ML — the equivalent inside Snowflake.
- Databricks AI Functions — the equivalent inside Databricks SQL.
- [[../frameworks/mindsdb|mindsdb]] — substrate-agnostic SQL-ML
  layer (works against MySQL/Postgres/Mongo/etc. as well as
  warehouses).
- pgvector / pg_ml — Postgres-native ML extensions.
- Vertex AI — Google's full ML platform; the place to escalate to
  when BigQuery ML isn't enough.

## Internal links

- [[README]] — bigquery section landing.
- [[../frameworks/mindsdb|mindsdb]] — the substrate-agnostic
  SQL-ML cousin.
- [[../frameworks/README|frameworks]] — broader ML frameworks hub.
- [[../time_series/README|ml/time_series]] — time-series methods
  (ARIMA+ context).

## Keywords

`#bigquery` `#bigquery-ml` `#sql-ml` `#dnn` `#arima` `#time-series`

## References / raw notes

### Google docs (canonical)

- **Introduction to BigQuery ML** —
  <https://cloud.google.com/bigquery-ml/docs/introduction>
  (start here: model-type matrix, pricing model, supported
  features.)
- **`CREATE MODEL` for DNN models** —
  <https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create-dnn-models>
  (DNN classifier / regressor syntax, hyperparameters, options.)
  Adjacent reference pages cover boosted trees, k-means, ARIMA+,
  matrix factorisation, AutoML tables, and remote (Vertex AI)
  models.
- **`CREATE MODEL` for time-series (`ARIMA_PLUS`)** —
  <https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create-time-series>
  Of particular interest:
  <https://cloud.google.com/bigquery-ml/docs/reference/standard-sql/bigqueryml-syntax-create-time-series#large_scale_time_series>
  — the *large-scale* variant for fitting many series at once.

### Tutorial / hands-on

- **BigQuery ML tutorial** (Rachael Tatman, Kaggle notebook) —
  <https://www.kaggle.com/code/rtatman/bigquery-machine-learning-tutorial/notebook>
  Worked end-to-end on the public Kaggle dataset; good for getting
  the syntax under your fingers.
