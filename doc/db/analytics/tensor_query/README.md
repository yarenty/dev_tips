---
title: Tensor query processing
keywords: [tensor-query, gpu, sql, query-processing, microsoft, polimi, research]
status: reviewed
---

# Tensor query processing

A small reading-list of papers on running SQL / relational queries as **tensor
programs on GPUs**. The core idea: instead of compiling SQL to traditional
operator pipelines, lower it to dense tensor operations (matmul, gather,
sort) and execute it on the same hardware ML workloads run on. The line of
work is mostly Microsoft Research (Matteo Interlandi & co.) plus
collaborations with Politecnico di Milano (Carlo Curino, Donghe Wang).

The interesting questions the papers tackle:

- which relational operators (joins, aggregates, sorts) admit clean tensor
  formulations and which need fallback paths;
- how to amortize GPU-transfer cost across queries;
- how this overlaps with — and competes against — modern columnar engines
  like DuckDB and Velox running on CPU.

## What's in this folder

- `2203.01877.pdf` — **Tensor Query Processor** (TQP), Interlandi et al.,
  arXiv 2022; the foundational "compile SQL to PyTorch" paper.
- `p3598-interlandi.pdf` — the **SIGMOD / VLDB** version of the TQP paper
  (the polished conference proceedings cut).
- `2211.02753v1.pdf` — follow-up arXiv paper extending TQP (operator
  coverage / optimization).
- `2405.09818v1.pdf` — 2024 follow-up (newer benchmarks / GPU-architecture
  work).
- `SEBD2023_Curino.pdf` — invited talk / paper from **SEBD 2023** (Italian
  database conference) by Carlo Curino summarising the line of work.
- `TensorQuery.pptx` — internal summary deck I made while reading these.
- `DONGHE_CV.pdf` — CV of one of the researchers (Donghe Wang); kept as
  context for the collaboration.

## External entry points

- [Tensor Query Processor on arXiv](https://arxiv.org/abs/2203.01877)
- [Microsoft Research — data systems group](https://www.microsoft.com/en-us/research/group/data-systems-group/)
- [Politecnico di Milano DEIB](https://www.deib.polimi.it/)

## Cross-section see-also

- [[db/analytics/README|analytics]] — sibling engine notes.
- [[db/analytics/datafusion/README|DataFusion]] — the CPU-side comparison
  point for any GPU SQL engine in 2024.

## Keywords

`#tensor-query` `#gpu` `#sql` `#research` `#microsoft-research` `#polimi`
