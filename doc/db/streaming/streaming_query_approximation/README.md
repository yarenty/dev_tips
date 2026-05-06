---
title: Streaming query approximation (AQP)
keywords: [aqp, approximate-query-processing, sketches, sampling, wavelets, streaming]
status: reviewed
---

# Streaming query approximation (AQP)

**Approximate Query Processing** (AQP) is the family of techniques for
answering aggregate queries over streams (and very large static datasets)
without scanning every row — using **sketches** (Count-Min, HyperLogLog,
AMS), **sampling**, or **wavelet** summaries. The core trade is *bounded
error* + provable confidence intervals in exchange for orders-of-magnitude
less storage, compute, and network bandwidth.

This folder collects foundational papers and a deeper write-up of the
Cormode & Garofalakis 2008 distributed-streams paper.

## What's in this folder

### Articles

- [[papers]] — paper-list / reading guide for AQP (Cormode & Garofalakis
  2008, Das thesis 2006, Liu encyclopedia entry, Li & Li 2018 survey).
- [[approximate_continuous_querying_over_distributed_streams]] — deeper
  notes on the Cormode & Garofalakis 2008 paper (continuous tracking of
  aggregates over distributed streams).

### Assets

- `img/` — figures pulled from the papers.
- `res/` — PDF copies of the source papers.

## External entry points

- [Cormode & Garofalakis (2008) — ACM TODS](https://doi.org/10.1145/1366102.1366106)
- [Li & Li (2018) — A Survey on AQP](https://link.springer.com/article/10.1007/s41019-018-0074-4)
- [Liu — Approximate Query Processing (Encyclopedia of Database Systems)](https://link.springer.com/referenceworkentry/10.1007/978-0-387-39940-9_534)
- [Apache DataSketches](https://datasketches.apache.org/) — the production
  sketch library you'd actually reach for today.

## Cross-section see-also

- [[db/streaming/README|streaming]] — parent section.
- [[druid]] — uses sketches (HLL, Theta, Quantiles) heavily for AQP-style
  queries.
- [[db/analytics/README|analytics]] — the offline OLAP counterpart.

## Keywords

`#aqp` `#approximate-query-processing` `#sketches` `#sampling` `#wavelets` `#streaming`
