---
title: GraphRAG — knowledge-graph-augmented retrieval
main_link: https://arxiv.org/abs/2404.16130
keywords: [graph-rag, graphrag, microsoft, knowledge-graph, community-summaries, lightrag, nano-graphrag]
status: reviewed
---

# GraphRAG — knowledge-graph-augmented retrieval

**Main link:** <https://arxiv.org/abs/2404.16130> (Edge et al. 2024, *From Local to Global: A GraphRAG Approach to Query-Focused Summarization*)

## Summary

GraphRAG is the family of RAG techniques that build (or reuse) a knowledge graph of entities and relations extracted from the corpus, then route queries against that graph instead of (or alongside) flat vector search. The branding crystallised around Microsoft Research's April 2024 paper, which introduced the now-canonical pipeline: chunk → LLM-extract entities and relations → cluster into communities → LLM-summarise each community → at query time pick *local* (entity-centric) or *global* (community-summary-aware) retrieval. The intent is to fix vanilla RAG's blind spot for **corpus-level questions** ("what are the main themes?") that no single chunk can answer.

## Insight

GraphRAG earns its keep on three query shapes vanilla RAG handles poorly: **multi-hop questions** (chains of relations between entities), **aggregative / "global" questions** ("summarise themes across the dataset"), and **entity-disambiguation queries** in document sets where the same name means different things. For straight factual lookups, vanilla RAG with a good reranker is usually faster and cheaper.

The ugly truth is **cost**: graph extraction is one LLM call per chunk (often more), so indexing a corpus with GraphRAG can cost 10–100× the dollars of a plain embedding index, plus weeks to a long-running batch. This is why a whole sub-genre of lighter implementations exists:

- **Microsoft GraphRAG** — the reference Python implementation (`microsoft/graphrag`); high quality but heavy.
- **nano-graphrag** — Jin Ying's stripped-down ~1k-LOC implementation; pedagogical and hackable.
- **LightRAG** (Hong et al. 2024, HKU) — dual-level (low/high) retrieval, faster incremental updates than Microsoft's batch-rebuild model.
- **Neo4j GraphRAG** — uses Neo4j as the graph + vector store; see [[neo4j_rag]].
- **LlamaIndex `PropertyGraphIndex`** and **LangChain `GraphCypherQAChain`** — framework-native variants.

Two pragmatic patterns worth knowing: (1) **don't extract a graph if you already have one** — if your domain already publishes a KG (UMLS, Wikidata, internal product catalogue), wire that in directly; extraction is a means, not an end. (2) **Hybrid is normal** — most production GraphRAG systems run vector search alongside graph traversal and rerank the union, rather than choosing one or the other.

## Similar / related topics

- **Vanilla RAG** — flat embeddings + top-k; baseline GraphRAG aims to beat on global/multi-hop queries.
- **KAG (Ant Group)** — schema-constrained, logic-form-guided alternative; see [[kag]].
- **HippoRAG** — neuroscience-inspired graph-traversal retrieval (Personalised PageRank over an extracted KG).
- **KG-augmented LLMs** — the broader research line (KG-COT, ToG, RoG); see [[../knowledge_graph/papers|kg/papers]].
- **Vector search alone** — when corpus is small or queries are factual, often beats GraphRAG on cost-per-quality.

## Internal links

<!-- reviewed -->
- [[README]] — RAG hub
- [[neo4j_rag]] — Neo4j-backed GraphRAG implementation
- [[kag]] — Ant Group's KAG: schema-constrained alternative
- [[agentic_rag]] — orthogonal axis; can wrap GraphRAG in an agent loop
- [[summary_of_best_practices]] — chunking / reranking foundations still apply
- [[../knowledge_graph/README|knowledge_graph]] — KG fundamentals
- [[../knowledge_graph/papers|kg/papers]] — KG-augmented-LLM reading list
- [[../llm/README|llm]] — LLM hub

## Keywords

`#graph-rag` `#graphrag` `#microsoft` `#knowledge-graph` `#lightrag` `#nano-graphrag` `#community-summaries`

## References / raw notes

- Microsoft GraphRAG paper (Edge et al. 2024): <https://arxiv.org/abs/2404.16130>
- Microsoft GraphRAG repo: <https://github.com/microsoft/graphrag>
- Microsoft Research blog: <https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/>
- nano-graphrag: <https://github.com/gusye1234/nano-graphrag>
- LightRAG (Hong et al. 2024): <https://arxiv.org/abs/2410.05779> · <https://github.com/HKUDS/LightRAG>
- HippoRAG: <https://arxiv.org/abs/2405.14831>
- LanceDB GraphRAG guide: <https://lancedb.github.io/lancedb/rag/graph_rag/>
- LlamaIndex `PropertyGraphIndex`: <https://docs.llamaindex.ai/en/stable/module_guides/indexing/lpg_index_guide/>
