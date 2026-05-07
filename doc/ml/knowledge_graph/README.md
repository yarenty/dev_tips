---
title: Knowledge graphs in ML
keywords: [knowledge-graph, kg, gnn, kg-embeddings, graphrag, petgraph]
status: reviewed
---

# Knowledge graphs in ML

Knowledge graphs (KGs) and graph-shaped data in ML / LLM pipelines: classical KG embeddings (TransE, RotatE, ComplEx) for link prediction, KG-aware GNNs (R-GCN, CompGCN), and the modern KG-augmented LLM / GraphRAG wave (GraphRAG, KAG, LightRAG, HippoRAG). This section also points at the *substrates* (Rust's `petgraph`, the embedding-side notes around Meta's data2vec).

## Articles

- [Knowledge graph research — curated reading list](papers.md) — three eras of KG-research papers (classical embeddings → KG-GNNs → KG-augmented LLMs) + survey-of-surveys + tooling pointers (PyKEEN, Connected Papers).
- [Meta's data2vec — towards a single self-supervised model](meta.md) — Meta's data2vec + doc2vec embedding-side notes (the encoder layer of a KG-augmented pipeline).
- [petgraph — Rust graph data-structure library](petgraph.md) — the canonical Rust graph data-structure crate (substrate, not learning).

## Picker shortcut

| If you need… | Reach for… |
|--------------|-----------|
| Link prediction over a static KG | Classical embeddings via **PyKEEN** (RotatE / ComplEx baselines) |
| Node / graph classification | **PyG** or **DGL** (R-GCN / CompGCN / GraphSAGE) |
| Multi-hop QA grounding an LLM in a corpus | **GraphRAG** / **LightRAG** / **HippoRAG-2** |
| Knowledge-augmented generation framework | **KAG** (OpenSPG) |
| Rust-native graph data structure + algorithms | **petgraph** |
| Rust query-engine *over* graphs / arbitrary data | **trustfall** (different layer; see Rust section) |

## Keywords

`#knowledge-graph` `#kg` `#kg-embeddings` `#graphrag` `#kag` `#lightrag` `#hipporag` `#petgraph` `#gnn`

## See also

- [[../fundamentals/graphnn|graphnn]] — GNN substrate (PyG / DGL / GCN / GAT).
- [[../rag/graph_rag|graph_rag]] — applied side: GraphRAG / LightRAG (P5.AE upcoming).
- [[../rag/kag|kag]] — KAG specifically.
- [[../rag/neo4j_rag|neo4j_rag]] — Neo4j as a KG substrate.
- [[../memory/episodic_memory|episodic_memory]] — KG-shaped agent memory (AriGraph, Zep).
- [[../llm/README|llm]] — LLM section (LLM-powered KG construction).
- [[programming/rust/data/trustfall|trustfall]] — Rust query engine over graphs (different layer from petgraph).
- [[db/nosql/README|db/nosql]] — graph-database substrates (Neo4j, ArangoDB, JanusGraph).
