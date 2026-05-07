---
title: xorq — multi-engine deferred ML pipelines on Ibis + DataFusion
main_link: https://github.com/xorq-labs/xorq
keywords: [xorq, letsql, datafusion, ibis, ml, pipelines, deferred, python]
status: reviewed
review_date: 2026/05/03
---

# xorq — multi-engine deferred ML pipelines on Ibis + DataFusion

**Main link:** <https://github.com/xorq-labs/xorq>

## Summary

**xorq** (formerly **LetSQL**, rebranded by xorq-labs in late 2024) is a Python deferred-computation framework that brings declarative-pipeline replicability and out-of-core performance to the ML / dataframe ecosystem. It sits on top of [Ibis](https://ibis-project.org/) (the polyglot dataframe API) and [Apache DataFusion](https://datafusion.apache.org/) (Rust columnar engine), letting you write pandas-style transformations that automatically cache intermediate results, never blow up memory, and seamlessly hop between SQL engines (DuckDB, DataFusion, Snowflake, etc.) and Python UDFs. The repo's flagship demo is the **SAM image-segmentation UDF** running on HuggingFace `candle` from inside a relational expression.

## Insight

xorq is the answer to: *what if your ML pipeline were a query plan?* The model is **deferred + multi-engine + cached** — you build an expression, the engine plans it (push-downs, predicate ordering, projection), and execution can dispatch each step to whichever backend is cheapest (DuckDB for joins, DataFusion for vectorised UDFs, Snowflake for warehouse-side filters, candle for inference). Reach for it when you've outgrown pandas-in-a-notebook but don't want to commit to Spark; or when a pipeline needs to be deployable as a small Rust binary rather than a 4 GB PyTorch container.

The pitch overlaps with [Ibis-on-DataFusion](https://ibis-project.org/) (xorq is essentially "Ibis + opinionated caching + Rust UDFs"), [Daft](https://www.getdaft.io/) (Python+Rust dataframe with multimodal/ML primitives), [Polars LazyFrame](https://pola.rs/), and Modin/Dask for the out-of-core angle. The differentiator is the explicit **caching layer** (`.cache(SourceStorage(...))`) and the Rust-side built-in UDFs that ship as part of the binary.

Caveats: xorq is **experimental and small**; the rebrand from LetSQL was disruptive (old `letsql` package and URLs still float around); the `candle` model coverage trails PyTorch substantially; and the multi-engine routing is more aspirational than transparent — you typically still pin one backend per pipeline.

## Similar / related topics

- **Ibis** — the polyglot dataframe API xorq embeds; runs against ~20 backends.
- **Daft** — Python+Rust distributed dataframe with ML/multimodal primitives, similar pitch.
- **Polars LazyFrame** — Rust dataframe with deferred execution, no multi-engine story.
- **DuckDB + `duckdb-ml`** — single-engine alternative for SQL-meets-ML.
- **Substrait** — cross-engine plan IR that makes multi-engine pipelines tractable.

## Internal links

- [[ml_letsql]] — the SAM-on-candle deep dive (xorq's flagship demo, written under the old name).
- [[README]] — DataFusion ecosystem landing.
- [[../README|rust/data]] — wider Rust data section.
- [[../../../../ml/tools/candle|candle]] — the Rust ML runtime xorq embeds.

## Keywords

`#xorq` `#letsql` `#datafusion` `#ibis` `#ml` `#pipelines` `#deferred` `#python`

## References / raw notes

- Repo: <https://github.com/xorq-labs/xorq>
- Original LetSQL post (SAM + candle): <https://www.letsql.com/posts/candle-image-segmentation/>
- Ibis: <https://ibis-project.org/>
- DataFusion: <https://datafusion.apache.org/>

> xorq is a deferred computational framework that brings the replicability and performance of declarative pipelines to the Python ML ecosystem. It enables us to write pandas-style transformations that never run out of memory, automatically cache intermediate results, and seamlessly move between SQL engines and Python UDFs — all while maintaining replicability. xorq is built on top of Ibis and DataFusion.
