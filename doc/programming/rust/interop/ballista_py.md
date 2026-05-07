---
title: Ballista (Python bindings)
main_link: https://github.com/apache/datafusion-ballista/tree/main/python
keywords: [ballista, datafusion, python, pyo3, distributed, arrow, big-data]
status: reviewed
---

# Ballista (Python bindings)

**Main link:** <https://github.com/apache/datafusion-ballista/tree/main/python>

## Summary

PyBallista is the Python binding layer for [Apache
Ballista](https://github.com/apache/datafusion-ballista) — the distributed
query-execution engine built on top of DataFusion. It exposes the same
DataFrame and SQL API as the single-node `datafusion` Python package but
submits the resulting physical plans to a Ballista scheduler that fans
work out across an executor cluster. The Python bindings now live inside
the main Ballista repo under `python/`; the previous standalone
`apache/datafusion-ballista-python` repo is archived in favour of that
location.

## Insight

This page sits at the intersection of three sibling stories worth keeping
straight:

- **[[db/distributed/ballista_distributed/README|Ballista]]** itself —
  the Rust scheduler/executor pair; the substrate.
- **[apache/datafusion-python](https://github.com/apache/datafusion-python)**
  — single-node Python wrapper around DataFusion. PyBallista is its
  distributed counterpart and shares much of the API surface.
- **PyO3 + maturin** — the binding/build pair under the hood (see
  [[pyo3|PyO3]], [[maturin]], [[to_python]]).

When you'd reach for it: you have a Python data-eng workflow (notebook,
Airflow, dbt-ish pipeline) that has outgrown a single Pandas/Polars
process and you'd rather spin up a small Ballista cluster on Kubernetes
than adopt full Spark/Dask. It's still niche — Ballista itself remains
less battle-tested than Spark — but the operational footprint is
dramatically smaller.

Things to know:

- **API parity is incomplete**: not every `datafusion-python` call has a
  Ballista equivalent yet; check the examples directory before committing.
- **Same Arrow IPC wire format**: results round-trip as Arrow record
  batches, so handing them to Pandas/Polars/PyArrow is essentially free.
- **Versioning lockstep**: PyBallista, Ballista, and DataFusion release
  together; pin all three.
- **Worked example of "expose a Rust SQL engine to Python"**: read this
  alongside [[to_python]] if you're authoring something similar.

## Similar / related topics

- [[db/distributed/ballista_distributed/README|Ballista]] — the
  distributed substrate.
- [[programming/rust/data/datafusion/README|DataFusion]] — the
  single-node engine Ballista distributes.
- [apache/datafusion-python](https://github.com/apache/datafusion-python)
  — single-node Python bindings.
- [Dask](https://www.dask.org/) / [Ray](https://www.ray.io/) — Python
  alternatives if "distributed Python compute" is the actual goal.
- [PySpark](https://spark.apache.org/docs/latest/api/python/) — the
  incumbent Python distributed-SQL workhorse.

## Internal links

<!-- reviewed -->

- [[pyo3|PyO3]] — the FFI binding crate underneath.
- [[maturin]] — wheel build & publish tool.
- [[to_python]] — the general "Rust → PyPI" recipe.
- [[db/distributed/ballista_distributed/README|Ballista]] — Rust
  substrate.
- [[programming/rust/data/datafusion/README|DataFusion]] — single-node
  engine.
- [[README]] — Rust interop landing.

## Keywords

`#ballista` `#datafusion` `#python` `#pyo3` `#distributed` `#arrow`

## References / raw notes

- [github.com/apache/datafusion-ballista — `python/`](https://github.com/apache/datafusion-ballista/tree/main/python)
  — current home of the bindings.
- [Old standalone repo (archived)](https://github.com/apache/datafusion-ballista-python)
  — README now redirects to the main repo.
- [Ballista user guide](https://datafusion.apache.org/ballista/) (Python
  examples included).
- [apache/datafusion-python](https://github.com/apache/datafusion-python)
  — sibling single-node Python bindings, useful as an API reference.
