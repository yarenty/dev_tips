---
title: MLCube — MLCommons' standard interface for containerised ML workloads
main_link: https://mlcommons.github.io/mlcube/
keywords: [mlcube, mlcommons, mlperf, containers, benchmarks, frameworks]
status: reviewed
---

# MLCube — MLCommons' standard interface for containerised ML workloads

**Main link:** <https://mlcommons.github.io/mlcube/>

## Summary

MLCube is an MLCommons (the people behind MLPerf) **convention** for
packaging an ML workload as a container with a standard interface
(metadata YAML + a small set of named tasks like `train` / `predict`
/ `download_data`). The same MLCube can then be run by any of the
MLCommons "runners" — locally with Docker / Singularity / Podman, on
Kubernetes, or on a cloud runner — with a single command. It is
**not** a new framework or service; think "Docker for ML benchmarks",
or a thin spec layer on top of containers.

## Insight

MLCube exists because reproducing other people's ML papers is
infamously painful — different framework versions, undocumented data
prep, hand-rolled launch scripts. By forcing every workload to expose
the same `mlcube.yaml` + a fixed task vocabulary, MLCommons can run
hundreds of submissions against the same set of runners, on the same
hardware, and produce the MLPerf leaderboards.

When to reach for MLCube:

- You're submitting to **MLPerf** (training or inference).
- You're publishing a model and want a "this is exactly how I ran
  it" packaging that survives your laptop dying.
- You're a research lab or hardware vendor that needs to run the
  same workload across many runtimes (bare metal / Docker / k8s /
  cloud) without rewriting the entry script each time.

When *not* to reach for it:

- You're just running your own ML pipeline in production — plain
  Docker + an orchestrator ([[langgraph]], Airflow, Kubeflow,
  Metaflow, Prefect) gets you there with less ceremony.
- You want experiment tracking / hyperparameter search / model
  registry — that's MLflow / Weights & Biases / Aim territory; MLCube
  is purely a packaging spec.

Honest framing: MLCube is **niche**. Outside the MLCommons /
MLPerf ecosystem and a few academic groups it sees little adoption.
The project itself notes it is "still in the very early stages" and
that hasn't really changed. If you're not running benchmarks, you
probably don't need it.

Gotchas:

- The runner ecosystem is uneven — local Docker is rock-solid, the
  k8s and cloud runners less so; check the matrix before committing.
- The task vocabulary (`download_data`, `train`, `predict`, …) is
  loose — submissions interpret it differently, which somewhat
  defeats the "drop-in interchangeable" pitch.
- Versioning is by container tag; reproducing a 2020-vintage MLCube
  often means hunting down old base images.

## Similar / related topics

- **MLPerf / MLCommons** — the benchmark suite MLCube was built for:
  <https://mlcommons.org/>
- **BentoML** — productionisation framework for serving ML models
  as containers; broader scope than MLCube.
- **Cog** (Replicate) — a more opinionated "package an ML model as
  a container" tool with a Python-first surface.
- **Kubeflow / Metaflow / Flyte / Prefect / Airflow** — full
  orchestration; MLCube is just the workload-packaging slice.
- **OCI image spec / Docker** — the substrate; MLCube adds metadata
  and a task convention on top.

## Internal links

<!-- reviewed -->

- [[README]] — frameworks landing.
- [[h2o]] — different angle on packaging (POJO/MOJO model export).
- [[../../kubernetes/README|kubernetes]] — common runner target.

## Keywords

`#mlcube` `#mlcommons` `#mlperf` `#containers` `#benchmarks`
`#frameworks`

## References / raw notes

- Site / docs: <https://mlcommons.github.io/mlcube/>
- Examples repo: <https://github.com/mlcommons/mlcube_examples>
- MLCommons (parent org): <https://mlcommons.org/>

### Project pitch (paraphrased)

MLCube brings the concept of *interchangeable parts* to ML models —
a shipping container that lets researchers and developers share the
software powering ML training and inference. It is a set of
conventions for ML software that can plug-and-play across systems:
local Docker, Singularity, Kubernetes, or a major cloud, all driven
by the same MLCube + an MLCommons-provided runner.

> *Note from upstream: this project is still in the very early
> stages and under active development; some parts may have
> unexpected/inconsistent behaviours.*

### Install

```sh
pip install mlcube
```

To uninstall:

```sh
pip uninstall mlcube
```

See the [examples repo](https://github.com/mlcommons/mlcube_examples)
and the [MLCube wiki](https://mlcommons.github.io/mlcube) for
hands-on tutorials.
