---
title: Knowledge graph research — curated reading list
main_link: https://github.com/totogo/awesome-knowledge-graph
keywords: [papers, knowledge-graph, kg, embeddings, transe, rotate, gnn, graphrag, kg-llm]
status: reviewed
---

# Knowledge graph research — curated reading list

**Main link:** <https://github.com/totogo/awesome-knowledge-graph>

## Summary

A curated reading list for the knowledge-graph thread of ML research, organised by era: the **classical KG embedding** generation (TransE, TransH, RotatE, DistMult, ComplEx — 2013-2019), the **KG-aware GNN** generation (R-GCN, CompGCN — 2017-2020), and the **modern KG-augmented LLM / GraphRAG** generation (HippoRAG, GraphRAG, KAG, LightRAG — 2023-2025). Use this as a starting point; for the *applied* RAG side, prefer [[../rag/graph_rag|graph_rag]].

## Insight

Three useful framings:

1. **Pick the era you actually need.** If you're doing link-prediction over a static KG (drug-target, recommender, citation) → classical embeddings or KG-GNNs. If you're grounding an LLM with a corpus that *has structure* → modern KG-augmented LLM / GraphRAG. The literatures don't talk to each other much; they're solving different problems.
2. **The 2024-2025 wave is a regression to GraphRAG.** Microsoft's GraphRAG paper (Edge et al. 2024) made it cool again to extract a KG from your corpus and use it for retrieval; LightRAG, KAG, HippoRAG-2 followed. The selling point is *multi-hop* questions that vanilla RAG struggles with. The cost is index complexity, latency, and LLM tokens during construction.
3. **Honest critique**: KG-augmented retrieval works *when the KG is right*, and getting the KG right is the hard part. For most projects, vanilla dense retrieval + reranker is still the right starting point; reach for graph methods when you've measured that vanilla isn't enough.

When this matters: medical / legal / scientific QA where multi-hop reasoning is the norm; entity-rich corpora (companies, drugs, legal entities, papers); recommendation; fraud / network analysis. When it doesn't: short-context QA, single-fact lookups, conversational chatbots without rich entity structure.

## Similar / related topics

- **awesome-knowledge-graph** — totogo's curated list (this article's anchor).
- **PyKEEN** — the de-facto Python KG-embedding training library; covers 40+ models.
- **OpenKE / GraphVite** — alternative embedding training stacks (older).
- **Microsoft GraphRAG** — Edge et al. 2024; the project that made the area hot again.
- **HippoRAG / HippoRAG-2** — neurobiologically-inspired graph-augmented continual learning.
- **KAG** — knowledge-augmented generation framework (OpenSPG / Ant Group, 2024).
- **LightRAG** — efficient dual-level retrieval over KGs (HKUDS 2024).

## Internal links

<!-- reviewed -->
- [[README]] — section landing.
- [[meta]] — sibling: Meta's data2vec / doc2vec embedding-side notes.
- [[petgraph]] — sibling: Rust graph data-structure library.
- [[../fundamentals/graphnn|graphnn]] — Graph Neural Networks landing (PyG / DGL / GCN / GAT — the GNN substrate for KG-GNN methods).
- [[../rag/graph_rag|graph_rag]] — applied side: GraphRAG / LightRAG / KAG.
- [[../rag/kag|kag]] — KAG specifically.
- [[../rag/neo4j_rag|neo4j_rag]] — Neo4j-as-substrate flavour.
- [[../memory/episodic_memory|episodic_memory]] — KG-shaped agent memory (AriGraph, Zep) overlaps with this literature.
- [[../llm/README|llm]] — LLM section.

## Keywords

`#papers` `#knowledge-graph` `#kg` `#embeddings` `#transe` `#rotate` `#gnn` `#graphrag` `#kg-llm`

## References / raw notes

### Curated landing points

- **awesome-knowledge-graph** — <https://github.com/totogo/awesome-knowledge-graph>. The main community list (datasets / tooling / papers / surveys).
- **PyKEEN** — <https://github.com/pykeen/pykeen>. The Python framework for training and evaluating KG embeddings; ~40+ models implemented.
- **Connected Papers** — <https://www.connectedpapers.com/>. Useful tool to explore neighbourhoods of a seed paper.
- **ResearchGate** — <https://www.researchgate.net/> as a search backstop.
- Original raw note: a [Connected Papers exploration](https://www.connectedpapers.com/main/9f7731d72e2aa251d2994eb1729c22aa78d0f718/) of "KG embedding for data mining vs. KG embedding for link prediction — two sides of the same coin?" — useful entry point if you want to see how the embedding-and-link-prediction literatures cluster.

### Classical KG embeddings (2013-2019)

- **TransE** — Bordes et al. 2013, *Translating Embeddings for Modeling Multi-relational Data.* Foundational; entities as vectors, relations as translations.
- **TransH** — Wang et al. 2014. Relation-specific hyperplanes.
- **DistMult** — Yang et al. 2015. Bilinear scoring.
- **ComplEx** — Trouillon et al. 2016. Complex-valued embeddings.
- **RotatE** — Sun et al. 2019, [arXiv 1902.10197](https://arxiv.org/abs/1902.10197). Relations as rotations in complex space; the modern strong baseline.
- **ConvE** / **TuckER** — convolutional and tensor-decomposition variants.

### KG-aware GNNs (2017-2020)

- **R-GCN** — Schlichtkrull et al. 2017. Relational Graph Convolutional Networks for KG link prediction and node classification.
- **CompGCN** — Vashishth et al. 2020. Composition-based message passing across relation types.
- **NBFNet** — Zhu et al. 2021. Neural Bellman-Ford for path-based reasoning.

### Modern KG-augmented LLMs (2023-2025)

- **GraphRAG** — Edge et al. (Microsoft) 2024 — *From Local to Global: A Graph RAG Approach to Query-Focused Summarization.* [arXiv 2404.16130](https://arxiv.org/abs/2404.16130). The work that made GraphRAG mainstream.
- **HippoRAG / HippoRAG-2** — Jiménez Gutiérrez et al. 2024-2025. Personalised PageRank over an entity-and-fact graph; biologically inspired.
- **KAG** — *Knowledge Augmented Generation* (OpenSPG / Ant Group 2024). <https://github.com/OpenSPG/KAG>.
- **LightRAG** — Guo et al. (HKUDS) 2024. Dual-level (low/high) retrieval over a co-occurrence KG; faster + cheaper than Microsoft GraphRAG.
- **Knowledge Driven Intelligence in Network Management Systems** (2021) — original raw-note pointer: <https://github.com/beikacao/blog/tree/main/2021-10-22>. KG-for-network-management application example.

### Surveys (good entry points)

- *A Survey of Graph Retrieval-Augmented Generation for Customised LLMs* — Zhang et al. 2025.
- *Graph Retrieval-Augmented Generation: A Survey* — Peng et al. 2024.
- *Unifying Large Language Models and Knowledge Graphs: A Roadmap* — Pan et al. 2023, [arXiv 2306.08302](https://arxiv.org/abs/2306.08302).
