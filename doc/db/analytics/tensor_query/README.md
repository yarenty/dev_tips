---
title: Tensor query processing
keywords: [tensor-query, gpu, sql, query-processing, microsoft, polimi]
status: reviewed
review_date: 2026/05/03
---

# Tensor query processing

A small reading-list on running SQL / relational queries as **tensor
programs on GPUs**. The core idea: instead of compiling SQL to traditional
operator pipelines, lower it to dense tensor operations (matmul, gather,
sort) and execute it on the same hardware ML workloads run on. The line of
work is mostly Microsoft Research (Matteo Interlandi & co.) plus
collaborations with Politecnico di Milano (Carlo Curino, Donghe Wang).

The interesting questions the work tackles:

- which relational operators (joins, aggregates, sorts) admit clean tensor
  formulations and which need fallback paths;
- how to amortize GPU-transfer cost across queries;
- how this overlaps with — and competes against — modern columnar engines
  like DuckDB and Velox running on CPU.

## Key references

- **Tensor Query Processor (TQP)**, Interlandi et al., arXiv 2022 — the
  foundational "compile SQL to PyTorch" paper
  ([arXiv:2203.01877](https://arxiv.org/abs/2203.01877)).
- The **SIGMOD / VLDB** version of TQP (Interlandi et al.) — the polished
  conference proceedings cut.
- A 2022 follow-up extending TQP
  ([arXiv:2211.02753](https://arxiv.org/abs/2211.02753)) on operator
  coverage and optimization.
- A 2024 follow-up
  ([arXiv:2405.09818](https://arxiv.org/abs/2405.09818)) with newer
  benchmarks and GPU-architecture work.
- Carlo Curino's invited talk at **SEBD 2023** (Italian database
  conference) summarising the line of work.

## External entry points

- [Tensor Query Processor on arXiv](https://arxiv.org/abs/2203.01877)
- [Microsoft Research — data systems group](https://www.microsoft.com/en-us/research/group/data-systems-group/)
- [Politecnico di Milano DEIB](https://www.deib.polimi.it/)

## Cross-section see-also

- [[db/analytics/README|analytics]] — sibling engine notes.
- [[db/analytics/datafusion/README|DataFusion]] — the CPU-side comparison
  point for any GPU SQL engine in 2024.

## Keywords

`#tensor-query` `#gpu` `#sql` `#microsoft-research` `#polimi`
