---
title: Graph Neural Networks (GNN)
main_link: https://pytorch-geometric.readthedocs.io/
keywords: [gnn, graph-neural-networks, pyg, dgl, gcn, gat, graphsage]
status: reviewed
review_date: 2026/05/03
---

# Graph Neural Networks (GNN)

**Main link:** <https://pytorch-geometric.readthedocs.io/>

## Summary

Graph Neural Networks combine deep learning with graph-structured data (nodes + edges + features
on either) by passing and aggregating messages along edges. The two canonical Python libraries are
**PyTorch Geometric (PyG)** — the larger, faster-moving ecosystem — and **Deep Graph Library
(DGL)** — multi-backend (PyTorch / TF / MXNet) and stronger on heterogeneous graphs. Real-world
applications include molecular property prediction (drug discovery, materials), social-network and
recommendation systems (link prediction, node classification), traffic and supply-chain
forecasting, fraud detection, and code-graph analysis.

## Insight

Reach for a GNN when the **structure of the connections genuinely carries signal** that a flat
feature vector would discard — molecular bonds, citation networks, social graphs, knowledge graphs,
program ASTs. If you can describe each entity with a fixed-length feature vector and the
relationships are mostly noise, a GBDT or MLP on flattened features will be cheaper, faster, and
often more accurate. Architecturally, four families cover ~95% of practical work:

- **GCN** (Kipf & Welling 2017) — the simplest and most-cited; spectral-graph-convolution baseline.
- **GAT** (Veličković et al. 2018) — adds attention to weight neighbours; usually the right modern default.
- **GraphSAGE** (Hamilton et al. 2017) — inductive (works on unseen nodes), neighbour-sampling for scalability.
- **Graph Transformers** (GraphGPS, Graphormer) — the attention-over-the-whole-graph era; SOTA on many benchmarks but expensive.

The two operational gotchas to remember: (1) **over-smoothing** — too many GNN layers make all node
embeddings collapse to similar vectors; deep GNNs are hard, 2-4 layers is normal. (2) **scaling**
— full-graph training on million-node graphs requires neighbour-sampling tricks (GraphSAINT,
ClusterGCN); naive PyG/DGL won't fit.

## Similar / related topics

- **PyTorch Geometric (PyG)** — the dominant Python GNN library; fast-moving, big tutorial set.
- **Deep Graph Library (DGL)** — Amazon-backed alternative; better story for heterogeneous graphs.
- **NetworkX** — classical (non-learned) graph analysis; useful for building inputs to a GNN.
- **Knowledge graphs / KGE** (TransE, ComplEx, RotatE) — adjacent: learning embeddings on relational graphs.
- **Graph databases** (Neo4j, ArangoDB, Memgraph) — where the graphs you'd feed a GNN often live.

## Internal links
- [[../knowledge_graph/README|knowledge_graph]] — sibling section: graph-structured knowledge representation.
- [[../frameworks/README|frameworks]] — DL frameworks (PyG/DGL build on PyTorch).
- [[pattern_learning]] — sibling parent topic; GNNs are pattern-learning over graphs.
- [[ml_auto_indexing_dbs]] — adjacent: ML inside the data layer.
- [[no_deep_learning_needed]] — counterpoint: when graph-structural signal is weak, a GBDT on hand-crafted features wins.

## Keywords

`#gnn` `#graph-neural-networks` `#pyg` `#dgl` `#gcn` `#gat` `#graphsage`

## References / raw notes

- [PyTorch Geometric](https://pytorch-geometric.readthedocs.io/) — canonical Python GNN library.
- [Deep Graph Library (DGL)](https://www.dgl.ai/) — multi-backend alternative.
- Google AI Blog: [*Robust Graph Neural Networks*](https://ai.googleblog.com/2022/03/robust-graph-neural-networks.html) (March 2022) — original auto-stub link, on adversarial-robustness for GNNs.
- Foundational papers:
  - GCN: [Kipf & Welling 2017](https://arxiv.org/abs/1609.02907).
  - GAT: [Veličković et al. 2018](https://arxiv.org/abs/1710.10903).
  - GraphSAGE: [Hamilton, Ying & Leskovec 2017](https://arxiv.org/abs/1706.02216).
  - Graphormer: [Ying et al. 2021](https://arxiv.org/abs/2106.05234).
- Survey: [Wu et al. 2021, *A Comprehensive Survey on Graph Neural Networks*](https://arxiv.org/abs/1901.00596).
- Course: [Stanford CS224W — Machine Learning with Graphs](http://web.stanford.edu/class/cs224w/) (Jure Leskovec).
