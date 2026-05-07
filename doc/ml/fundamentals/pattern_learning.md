---
title: Pattern learning — overview
main_link: https://hastie.su.domains/ElemStatLearn/
keywords: [pattern-learning, pattern-recognition, sequence-models, anomaly-detection, association-rules]
status: reviewed
review_date: 2026/05/03
---

# Pattern learning — overview

**Main link:** <https://hastie.su.domains/ElemStatLearn/>

## Summary

A meta-topic landing page covering "learning patterns from data" as a broad area — the parent
discipline that includes sequence models, motif discovery, association-rule mining, time-series
pattern recognition, anomaly detection, and structural-pattern learning over graphs and sequences.
The canonical reference is Hastie, Tibshirani & Friedman's *The Elements of Statistical Learning*
(free PDF, 2nd ed.), which covers the statistical core of the field; Bishop's *Pattern Recognition
and Machine Learning* covers the same ground with a Bayesian flavour.

## Insight

"Pattern learning" is more useful as an organising concept than as a specific technique — almost all
of supervised and unsupervised ML can be cast as pattern recognition. The page exists so siblings
like `[[../time_series/README|time_series]]`, `[[../knowledge_graph/README|knowledge_graph]]`,
`[[../genetics/README|genetics]]` (sequence-pattern discovery), and the anomaly-detection corner of
`[[../tools/README|tools]]` have somewhere to point as a parent. When picking a *concrete* technique
the right axis is usually the **shape of the data** (i.i.d. tabular vs. ordered sequence vs. graph
vs. spatio-temporal) and **whether labels exist** (supervised classification vs. unsupervised
clustering / motif mining / outlier scoring). The deep-learning incumbents (Transformers for
sequences, GNNs for graphs, CNNs for spatial data) have absorbed most of the academic attention
since ~2017, but the classical pattern-mining literature (frequent-itemset / FP-growth / sequential-
pattern mining / DTW) still wins on small datasets, interpretability, and explainable rules.

## Similar / related topics

- **Time-series analysis** — patterns over a temporal axis (forecasting, motif discovery, change-point detection).
- **Sequence models** (RNN/LSTM/Transformer) — the deep-learning workhorse for ordered data.
- **Association-rule mining** (Apriori, FP-growth) — classical "people who bought X also bought Y" patterns.
- **Anomaly / outlier detection** — pattern learning's negative-space companion.
- **Graph pattern mining / motif discovery** — patterns in graph topology, adjacent to `graphnn` for the learned-representation version.

## Internal links
- [[../time_series/README|time_series]] — sub-area: patterns over time.
- [[../knowledge_graph/README|knowledge_graph]] — sub-area: patterns in graph-structured knowledge.
- [[../genetics/README|genetics]] — sibling: sequence-pattern discovery in DNA/RNA.
- [[graphnn]] — the GNN angle on graph-pattern learning.
- [[classification]] — sibling on supervised pattern-labelling.
- [[no_deep_learning_needed]] — counterpoint: when classical pattern-mining still wins.

## Keywords

`#pattern-learning` `#pattern-recognition` `#sequence-models` `#anomaly-detection` `#association-rules`

## References / raw notes

- [Hastie, Tibshirani & Friedman, *The Elements of Statistical Learning* (free PDF)](https://hastie.su.domains/ElemStatLearn/) — the canonical statistical-learning reference.
- [Bishop, *Pattern Recognition and Machine Learning* (free PDF)](https://www.microsoft.com/en-us/research/people/cmbishop/prml-book/) — Bayesian-flavoured companion.
- [Han, Kamber & Pei, *Data Mining: Concepts and Techniques*](https://www.elsevier.com/books/data-mining-concepts-and-techniques/han/978-0-12-381479-1) — classical pattern-mining textbook.
- [scikit-learn user guide — *Unsupervised learning*](https://scikit-learn.org/stable/unsupervised_learning.html) — practical entry point.
