---
title: ML for automatic database indexing & tuning
main_link: https://arxiv.org/abs/1712.01208
keywords: [learned-indexes, auto-tuning, query-optimisation, db, ml-for-systems]
status: reviewed
---

# ML for automatic database indexing & tuning

**Main link:** <https://arxiv.org/abs/1712.01208>

## Summary

"ML for systems" is the umbrella for using learned models inside the database engine itself —
replacing or augmenting hand-engineered structures (B-trees, hash indexes), query optimisers,
configuration knobs, and partitioning decisions with models trained on real workloads. The
foundational paper is Kraska et al. 2018 *The Case for Learned Index Structures* (Google), which
showed that a learned recursive-model index can sometimes beat a B-tree on lookup time and memory
for read-heavy workloads. The area has since branched into learned cardinality estimation
(Neo, Bao), automated knob tuning (OtterTune, CDBTune), workload-aware partitioning, and learned
query optimisation.

## Insight

The honest 2025 picture: **research is exciting, production adoption is slow.** Production OLTP
systems still default to B-trees / LSM-trees because their worst-case behaviour is well-understood,
they handle inserts/updates cheaply, they don't need retraining when the data distribution shifts,
and they have decades of operational tooling around them. Learned indexes shine on **static or
slowly-changing read-heavy** workloads with predictable key distributions (analytical stores,
sorted columnar data, time-series). Auto-tuning is the area where ML has had the most real-world
impact — OtterTune-style tools genuinely help operators pick `shared_buffers` /
`effective_cache_size` / etc. without weeks of benchmarking. The gotchas to keep in mind:
training-data drift, the cold-start problem (a fresh DB has no workload to learn from), explainability
(DBAs want to know *why* a plan was chosen), and the operational cost of a model-retraining pipeline
inside an already-complex system. Watch the area, but don't bet your OLTP indexes on it yet.

## Similar / related topics

- **B-trees / LSM-trees** — the incumbents learned indexes are pitched against.
- **Adaptive query optimisation** (Postgres, SQL Server) — the older, statistics-based version of "learn from the workload".
- **AutoML for tabular models** — sibling field; same "let a model pick the model" idea, applied above the DB instead of inside it.
- **Vector indexes** (HNSW, IVF) — adjacent area where learned structure already won (vector DBs are essentially learned-index-shaped).
- **Cost-based optimisers** — what learned cardinality estimators (Neo, Bao) are trying to replace.

## Internal links
- [[../../db/README|db]] — the database section this work targets.
- [[../../db/relational/README|db/relational]] — where B-tree-replacement matters most.
- [[../../db/vector/README|db/vector]] — sibling area where learned structure is already mainstream.
- [[no_deep_learning_needed]] — sibling on classical-vs-deep tradeoffs (relevant: most ML-for-DB work uses small models, not deep nets).
- [[pattern_learning]] — sibling on learning patterns from data.

## Keywords

`#learned-indexes` `#auto-tuning` `#query-optimisation` `#db` `#ml-for-systems`

## References / raw notes

- Foundational: [Kraska et al. 2018, *The Case for Learned Index Structures*](https://arxiv.org/abs/1712.01208) (SIGMOD'18, Google).
- Auto-tuning: [OtterTune (CMU, 2017)](https://ottertune.com/), [CDBTune](https://dl.acm.org/doi/10.1145/3299869.3300085).
- Learned query optimisation: [Bao (MIT, 2020)](https://arxiv.org/abs/2004.03814), [Neo (MIT, 2019)](https://arxiv.org/abs/1904.03711).
- Survey: [*Machine Learning for Databases* (Li et al., 2021)](https://arxiv.org/abs/2105.06127).
- ALEX (updatable learned index): [Ding et al. 2020](https://arxiv.org/abs/1905.08898).
