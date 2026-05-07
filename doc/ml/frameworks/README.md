---
title: ML frameworks — agent orchestration, AutoML, packaging
keywords: [ml, frameworks, agents, automl, sql-ml, mlcube]
status: reviewed
---

# ML frameworks — agent orchestration, AutoML, packaging

This section covers **AI/ML orchestration and packaging frameworks** —
the layer that sits *above* a model runtime ([[../llm/runtimes/README|llm/runtimes]],
candle, PyTorch) and helps you build, ship and operate an
application around models. Specific neural-network libraries
(PyTorch, TensorFlow, candle, burn, etc.) live elsewhere — see
[[../tools/README|ml/tools]] for the broader ML tooling hub and
[[programming/rust/ml/README|programming/rust/ml]] for the Rust-side
landscape.

## Articles

### Agent / LLM orchestration

- [DSPy](dspy.md) — Stanford's declarative LLM-programming framework
  that **compiles** prompts and weights from a metric instead of
  hand-tuning prompt strings.
- [LangGraph](langgraph.md) — LangChain's spinout for **stateful,
  cyclic** agent workflows with checkpointing and human-in-the-loop.
- [phidata / agno](phidata.md) — batteries-included agent + RAG
  framework with a built-in playground UI and monitoring.

### AutoML / SQL-ML

- [H2O.ai](h2o.md) — the canonical open-source AutoML platform for
  tabular data; JVM-backed with R/Python/Scala bindings.
- [MindsDB](mindsdb.md) — ML as **virtual tables** on top of your
  existing database — `CREATE MODEL ... PREDICT ...` over MySQL /
  Postgres / Mongo / Snowflake.

### Workload packaging

- [MLCube](mlcube.md) — MLCommons / MLPerf's "Docker for ML
  benchmarks" — a thin spec for packaging an ML workload as a
  cross-runtime portable container.

### Rust-side ML

The frameworks here are mostly Python or JVM. For the Rust-language
ML library landscape (linfa / candle / burn / tch-rs / dfdx /
big-brain), see the dedicated hub:

- [[programming/rust/ml/README|programming/rust/ml]] — Rust ML
  libraries landing.
- [[programming/rust/ml/ml_in_rust|programming/rust/ml/ml_in_rust]]
  — opinionated picker (which crate for which job, what to avoid).

## Decision shortcut

| You want to…                                              | Reach for                       |
|-----------------------------------------------------------|---------------------------------|
| Compile prompts/weights from a metric (no hand-tuning)    | [[dspy]]                        |
| Build a cyclic, stateful agent with checkpoints + HITL    | [[langgraph]]                   |
| Ship an agent / RAG fast with built-in monitoring         | [[phidata]] (agno)              |
| AutoML on tabular data, export model as JVM JAR           | [[h2o]]                         |
| Add predictions to a SQL DB with `CREATE MODEL`           | [[mindsdb]]                     |
| Submit to MLPerf / package an ML workload reproducibly    | [[mlcube]]                      |
| Classical ML in a Rust service (no Python)                | [[programming/rust/ml/linfa\|linfa]] |
| Inference of HuggingFace / PyTorch models in Rust         | `candle` / `burn` (see Rust hub) |

## Keywords

`#ml` `#frameworks` `#agents` `#automl` `#sql-ml` `#mlcube`

## See also

- [[../README|ml]] — broader ML hub.
- [[../tools/README|ml/tools]] — ML tooling (notebooks, registries,
  serving, ETL).
- [[../llm/README|llm]] — LLM landing (models, runtimes, prompts).
- [[../agents/README|agents]] — agent orchestration patterns.
- [[../rag/README|rag]] — retrieval-augmented generation.
- [[../mcp/README|mcp]] — Model Context Protocol.
- [[../finetuning/README|finetuning]] — model fine-tuning.
- [[../bigquery/README|bigquery]] — SQL-ML in BigQuery (the
  warehouse-native cousin of MindsDB).
- [[programming/rust/ml/README|programming/rust/ml]] — Rust ML
  libraries hub.
