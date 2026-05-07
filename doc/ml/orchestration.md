---
title: ML / data orchestration — Airflow, Prefect, Dagster, Kestra, Argo, Metaflow, Flyte, ZenML
main_link: https://airflow.apache.org/
keywords: [orchestration, workflow, pipeline, airflow, prefect, dagster, kestra, argo, kubeflow, metaflow, flyte, zenml, hamilton, mlops]
status: reviewed
---

# ML / data orchestration — Airflow, Prefect, Dagster, Kestra, Argo, Metaflow, Flyte, ZenML

**Main link:** <https://airflow.apache.org/>

## Summary

Once you have **more than one batch job** — data ingestion, feature engineering, model training, batch scoring, evaluation, retraining — you have a workflow-orchestration problem: how to define the dependency graph, how to schedule it, how to retry on failure, how to backfill, how to pass data between steps, how to run on a Kubernetes cluster vs your laptop, and how to observe the whole thing. The dominant answer for ~a decade was **Apache Airflow** (Airbnb, 2014); the modern field includes **Prefect** (Pythonic, hybrid cloud), **Dagster** (asset-aware), **Kestra** (YAML-first, no Python required), **Argo Workflows** + **Kubeflow Pipelines** (Kubernetes-native), **Metaflow** (Netflix's pythonic abstraction), **Flyte** (Lyft's typed-DSL, k8s-native), **ZenML** (ML-pipelines on top of other orchestrators), and library-not-platform tools like **Hamilton** and **Ploomber**.

> **Note on naming.** This article covers **workflow / pipeline / data-and-ML orchestration** at the *job-level* (Airflow shape). For *agent control flow* — LangGraph-style runtime graphs of LLM calls — see `[[agents/orchestration|agents/orchestration]]`. Same word, very different shape: this section's tools schedule batch DAGs of containerised jobs that run for minutes to hours; the agent-orchestration article covers in-process Python loops with shared state that run per-request.

## Insight

**The picker matrix.** No single tool dominates; the right pick is shaped by *who writes pipelines* (Python data engineers / data scientists / SREs / non-coders), *where they run* (laptop / Kubernetes / a SaaS / Snowflake), and *how stateful the unit of work is* (task / dataset / asset).

| Tool | Sweet spot | Authoring | Runtime |
|---|---|---|---|
| **Apache Airflow** | Legacy default; huge ecosystem of operators; Big-Tech-tested | Python DAGs (`@dag` / `@task` decorators in 2.x) | Self-hosted; MWAA / Cloud Composer / Astronomer hosted |
| **Prefect 3.x** | Modern Pythonic Airflow replacement; dynamic DAGs without DAG-objects | Plain Python functions + `@flow` / `@task` | Prefect Cloud (free tier) + self-hosted workers ("hybrid") |
| **Dagster** | **Asset-aware** — model the *data products*, not the tasks; software-defined assets | Python; `@asset` / `@op` / `@graph` | Dagster Cloud + OSS; runs on K8s, Docker, ECS |
| **Kestra** | **YAML-first**, no Python required; non-coder-friendly; plug-in ecosystem | YAML flows + scriptable plug-ins | Self-hosted (Java); Kestra Cloud |
| **Argo Workflows** | **Kubernetes-native**, container-per-step; the substrate under Kubeflow | YAML CRDs (Workflow, WorkflowTemplate) | Kubernetes-only |
| **Kubeflow Pipelines (KFP)** | ML-pipelines on Argo or Tekton; Vertex AI Pipelines uses KFP | Python DSL → compiled YAML | K8s; Vertex AI Pipelines (managed GCP) |
| **Metaflow** | Netflix's "data scientist productivity" framework; great cards/UI; pythonic tier | Python `FlowSpec` classes with `@step` | Local → AWS Batch / Step Functions / K8s; Outerbounds (managed) |
| **Flyte** | Lyft-origin, **typed Python DSL**, strong caching/versioning, k8s-native | Python with type hints (FlytePropeller compiles) | Kubernetes-only; Union (managed) |
| **ZenML** | **ML pipelines on top of** another orchestrator; portable across stacks | Python `@pipeline` / `@step`; pluggable backends | Backend = Airflow / Kubeflow / Vertex / SageMaker / Azure ML / local |
| **Hamilton** | **Library, not platform**: DAG-of-pure-Python-functions, runs anywhere | Plain Python functions with naming conventions | Anywhere — call from a script, Airflow task, FastAPI handler |
| **Mage** | Notebook-style data pipelines, dbt integration, lower-code | Python / SQL / R blocks in a UI | Self-hosted; Mage Pro |
| **Ploomber** | Notebook-and-script ML pipelines; Jupyter-native | YAML `pipeline.yaml` + scripts/notebooks | Self-hosted; was acquired/wound down — check status |
| **Temporal** | Durable execution, not strictly "DAG"; long-running, retry-safe workflows | Python / Go / Java / TypeScript SDKs | Self-hosted + Temporal Cloud |
| **Step Functions / ADF / Azure Synapse Pipelines** | Hyperscaler-native; you're already deep in AWS / Azure | JSON state machines / drag-and-drop | Managed only |
| **dbt + Airflow / Dagster** | Pure SQL transforms; orchestrator just schedules `dbt run` | SQL + YAML | Whatever you point it at |

**Decision shortcuts.**

- **Already have Airflow?** Stay unless it's actively painful. Migration cost is large.
- **Greenfield Python team, want modern ergonomics?** **Prefect** or **Dagster**. Prefect is closer to "just decorate Python functions"; Dagster is closer to "model your data products and the system reasons about freshness".
- **All-in on Kubernetes?** **Argo Workflows** for the substrate; **Kubeflow Pipelines** if you want the ML-specific DSL on top; **Flyte** if you want strong typing + caching baked in.
- **Non-engineers writing flows?** **Kestra** (YAML) or **Mage** (notebook UI).
- **Want to stay portable across orchestrators?** **ZenML** as the abstraction layer; pick the backend per environment.
- **You don't actually need an orchestrator, just a DAG library?** **Hamilton** — write functions with consistent naming, get a DAG, run it anywhere.
- **Your "workflow" is event-driven, long-running, durable, retries-mid-flight?** **Temporal** (different category — durable execution, not batch scheduling).

**Honest gotchas.**

- **Airflow's "Pythonic" reputation is misleading.** The `@task` / TaskFlow API in 2.x is genuinely nice, but the operator-and-XCom heritage still bleeds through, and dynamic DAG generation has historically been awkward. If a fresh team picks Airflow today purely on familiarity, that's mostly inertia.
- **Asset-orientation vs task-orientation matters more than people admit.** Dagster's bet is that "the *table* you produced" is a more useful unit than "the *task* that ran". If your team thinks in datasets/features, Dagster's mental model fits; if they think in jobs/cron, Airflow's fits.
- **Kubernetes-native ≠ free.** Argo / KFP / Flyte give you per-step containers with isolation and scaling, but you pay in YAML, debugging-via-`kubectl describe`, and a non-trivial cluster operator's job.
- **"Hybrid cloud" can mean different things.** Prefect's hybrid model runs control plane in their cloud and workers in your VPC (data never leaves your network). Dagster Cloud has a similar model. Verify which side runs which before assuming compliance.
- **Backfills are still hard.** Every orchestrator claims to do them; doing them *correctly* across late-arriving data, schema changes, and downstream cache invalidation is hard regardless of tool.
- **Don't conflate orchestrators with experiment trackers.** MLflow / Weights & Biases / Comet / Neptune track *runs* and *metrics*; orchestrators schedule *jobs*. They compose; one doesn't replace the other.
- **CI/CD for pipelines is still under-tooled.** "How do I deploy a new DAG version without breaking the running one?" — most orchestrators leave this to you.

**Where this fits in the stack.**

```text
            ┌─────────────────────────────────────────────────┐
            │  Experiment tracking: MLflow, W&B, Comet        │
            ├─────────────────────────────────────────────────┤
            │  Orchestrator: Airflow / Prefect / Dagster /    │
            │                Argo / KFP / Flyte / Metaflow    │
            ├─────────────────────────────────────────────────┤
            │  Compute: K8s / EMR / Databricks / Snowpark /   │
            │           Ray / Spark / SageMaker / Vertex      │
            ├─────────────────────────────────────────────────┤
            │  Storage: S3/GCS/ADLS, Delta/Iceberg/Hudi,      │
            │           Postgres, Snowflake, BigQuery         │
            └─────────────────────────────────────────────────┘
```

The orchestrator is the **scheduler + dependency graph + observability layer**, not the compute. A modern stack usually has all four layers as separate concerns; if the orchestrator is also doing the computation in-process, that's fine for small jobs but doesn't scale.

## Similar / related topics

- **Apache Airflow** — the legacy default, biggest operator ecosystem, hosted as MWAA / Cloud Composer / Astronomer.
- **Prefect 3.x** — modern Pythonic Airflow replacement; hybrid cloud; very dynamic-DAG-friendly.
- **Dagster** — asset-aware; software-defined assets are the differentiator.
- **Kestra** — YAML-first, plug-in ecosystem; appealing to non-Python teams.
- **Argo Workflows** — Kubernetes-native CRDs; the substrate under Kubeflow Pipelines.
- **Kubeflow Pipelines** — ML-DSL on top of Argo / Tekton; **Vertex AI Pipelines** is the managed-GCP variant.
- **Metaflow** — Netflix's data-scientist-friendly Python framework with cards/UI; Outerbounds is the managed version.
- **Flyte** — Lyft-origin typed-Python DSL on K8s; strong caching and versioning; Union is the managed version.
- **ZenML** — the orchestrator-of-orchestrators; portable pipelines across Airflow / KFP / Vertex / SageMaker / Azure ML.
- **Hamilton** — DAG-of-functions library, not a platform; runs anywhere Python runs.
- **Mage** — notebook-style data pipelines with a low-code UI.
- **Temporal** — durable-execution workflows; different category from batch scheduling but often considered alongside.
- **Step Functions / Azure Data Factory / Azure Synapse Pipelines** — hyperscaler-native managed orchestration.
- **dbt** — SQL transforms; almost always wrapped by one of the orchestrators above.

## Internal links

<!-- reviewed -->
- [[agents/orchestration|agents/orchestration]] — the *agent-control-flow* sense of "orchestration" (LangGraph & friends); same word, very different shape.
- [[frameworks/README|frameworks]] — ML frameworks (DSPy, LangGraph, phidata, MindsDB, H2O) that often run *inside* an orchestrated pipeline.
- [[../db/streaming/cdc|cdc]] — Change Data Capture, the streaming-side counterpart that often feeds these batch pipelines.
- [[../db/distributed/scheduling/README|db/distributed/scheduling]] — the broader scheduling theory (data locality, work stealing, gang scheduling) underneath these tools.
- [[README|ml]] — the parent ML hub.
- [[bigquery/README|bigquery]] — BigQuery ML pipelines often live inside one of these orchestrators.
- [[finetuning/README|finetuning]] — fine-tuning runs are a very common payload for these pipelines.

## Keywords

`#orchestration` `#workflow` `#pipeline` `#airflow` `#prefect` `#dagster` `#kestra` `#argo` `#kubeflow` `#metaflow` `#flyte` `#zenml` `#hamilton` `#mlops` `#scheduling`

## References / raw notes

Canonical entry points per tool:

- Apache Airflow: <https://airflow.apache.org/> · GitHub: <https://github.com/apache/airflow>
- Prefect: <https://www.prefect.io/> · GitHub: <https://github.com/PrefectHQ/prefect>
- Dagster: <https://dagster.io/> · GitHub: <https://github.com/dagster-io/dagster>
- Kestra: <https://kestra.io/> · GitHub: <https://github.com/kestra-io/kestra>
- Argo Workflows: <https://argoproj.github.io/workflows/>
- Kubeflow Pipelines: <https://www.kubeflow.org/docs/components/pipelines/>
- Vertex AI Pipelines: <https://cloud.google.com/vertex-ai/docs/pipelines/introduction>
- Metaflow: <https://metaflow.org/> · GitHub: <https://github.com/Netflix/metaflow>
- Outerbounds (managed Metaflow): <https://outerbounds.com/>
- Flyte: <https://flyte.org/> · GitHub: <https://github.com/flyteorg/flyte>
- Union (managed Flyte): <https://www.union.ai/>
- ZenML: <https://www.zenml.io/> · GitHub: <https://github.com/zenml-io/zenml>
- Hamilton: <https://hamilton.dagworks.io/> · GitHub: <https://github.com/dagworks-inc/hamilton>
- Mage: <https://www.mage.ai/> · GitHub: <https://github.com/mage-ai/mage-ai>
- Ploomber: <https://github.com/ploomber/ploomber>
- Temporal: <https://temporal.io/>
- AWS Step Functions: <https://aws.amazon.com/step-functions/>
- Azure Data Factory: <https://learn.microsoft.com/en-us/azure/data-factory/>
- dbt: <https://www.getdbt.com/>

Comparison reads:

- "A Brief History of Workflow Orchestration": <https://www.prefect.io/blog/airflow-to-prefect-why-modern-orchestration-needs-a-new-approach> (vendor blog, take with salt — useful for the historical framing)
- Astronomer's *State of Airflow* surveys (annual): <https://www.astronomer.io/state-of-airflow/>
- Dagster's "Modern Data Stack" framing (asset-orientation pitch): <https://dagster.io/blog/dagster-and-the-modern-data-stack>
- Awesome Workflow Engines list: <https://github.com/meirwah/awesome-workflow-engines>
