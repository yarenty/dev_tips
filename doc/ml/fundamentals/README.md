---
title: ML fundamentals
keywords: [fundamentals, ml, classical-ml, deep-learning, learning-resources]
status: reviewed
review_date: 2026/05/03
---

# ML fundamentals

This section covers **ML concepts and learning approaches** — the *what* and the *why*, not the
*which framework*. For specific deep-learning frameworks see `[[../frameworks/README|frameworks]]`;
for domain applications see `[[../time_series/README|time_series]]`,
`[[../genetics/README|genetics]]`, `[[../knowledge_graph/README|knowledge_graph]]`,
`[[../voice/README|voice]]`, etc.; for the LLM/RAG/agent stack see
`[[../llm/README|llm]]`, `[[../rag/README|rag]]`, `[[../agents/README|agents]]`.

The articles below are organised thematically rather than alphabetically — pick the cluster that
matches your question.

## Decision shortcut — "where do I start?"

| If your question is... | Read |
|------------------------|------|
| What course should I take to learn ML/DL? | [[courses]] |
| I have a tabular dataset, do I need deep learning? | [[no_deep_learning_needed]] |
| My target labels have an *order* (1-5 stars, severity) | [[classification]] |
| I want to cluster numeric data | [[kmeans]] |
| My data is graph-shaped (molecules, social, citations) | [[graphnn]] |
| I have no labels for the classes I care about | [[zero_shot]] |
| What was that "MLP-replacement" thing from 2024? | [[kan]] |
| I need to delete training data after the fact | [[unlearning]] |
| Is there a meta-topic for "learning patterns from data"? | [[pattern_learning]] |
| ML inside the database engine itself | [[ml_auto_indexing_dbs]] |

## Core supervised / unsupervised learning

- [Classification — ordinal targets](classification.md) — the niche of order-aware classification (CORN/CORAL, dlordinal).
- [k-means clustering](kmeans.md) — the canonical unsupervised clustering algorithm; gotchas and when to reach for DBSCAN/GMM/HDBSCAN instead.
- [Graph Neural Networks (GNN)](graphnn.md) — deep learning over graph-structured data; PyG vs DGL, GCN/GAT/GraphSAGE.

## Reading lists

- [Top ML / DL learning resources](courses.md) — Karpathy's *Zero to Hero*, fast.ai, Andrew Ng, MIT 6.S191, CS231n/CS224N, plus curated resource repos.

## Modern ideas worth knowing

- [KAN — Kolmogorov-Arnold Networks](kan.md) — the 2024 MLP-alternative that briefly broke ML Twitter; honest take on what it's good for.
- [Zero-shot learning](zero_shot.md) — classifying without labelled examples; CLIP for vision, LLMs for NLP.
- [Machine unlearning](unlearning.md) — removing a training example's influence; SISA, GDPR, federated unlearning.

## Niche / contrarian

- [When you don't need deep learning](no_deep_learning_needed.md) — the well-documented cases where GBDTs and classical ML still win.
- [Pattern learning — overview](pattern_learning.md) — meta-topic landing for the broad area of "learning patterns from data".

## Cross-domain

- [ML for automatic database indexing & tuning](ml_auto_indexing_dbs.md) — learned indexes (Kraska et al. 2018), auto-tuning (OtterTune), the gap between research and production.

## Keywords

`#fundamentals` `#ml` `#classical-ml` `#deep-learning` `#learning-resources`

## See also

- [[../README|ml]] — section landing.
- [[../frameworks/README|frameworks]] — DL framework choice (PyTorch / JAX / TF / Keras).
- [[../tools/README|tools]] — ML tooling, AutoML, experiment tracking, MLOps.
- [[../llm/README|llm]] — large language models.
- [[../data/README|data]] — datasets, data engineering for ML.
- [[programming/rust/ml/linfa|programming/rust/ml/linfa]] — the canonical "scikit-learn for Rust"; ships k-means, ordinal/linear models, and more.
