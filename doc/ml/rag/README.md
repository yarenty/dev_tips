---
title: RAG — retrieval-augmented generation
keywords: [rag, retrieval, embeddings, hybrid-search, graph-rag, agentic-rag, knowledge-graph]
status: reviewed
review_date: 2026/05/03
---

# RAG — retrieval-augmented generation

**Retrieval-Augmented Generation (RAG)** is the pattern of fetching relevant context from your own knowledge base at query time and stuffing it into an LLM's prompt, instead of relying solely on the model's parametric memory. It was named in the [Lewis et al. 2020 paper](https://arxiv.org/abs/2005.11401) (Facebook AI), and by 2023 it had become the dominant production pattern for grounding LLMs in private data — reducing hallucinations, providing citations, and side-stepping the cost and freshness problems of fine-tuning a model every time the knowledge changes.

The 2024–2025 landscape has fanned out from "vanilla" RAG (one embedding lookup, top-k chunks into the prompt) into several axes that compose: **query preprocessing** (HyDE, multi-query, decomposition) sits *upstream* of retrieval; **agentic loops** (Self-RAG, CRAG, Adaptive-RAG) wrap retrieval in an LLM controller; **graph-shaped retrieval** (Microsoft GraphRAG, KAG, LightRAG, Neo4j) replaces or augments vector search with a knowledge graph; and across all of them the **production basics** — chunking, hybrid BM25+dense, reranking, evaluation — keep mattering more than any one fancy technique.

## How to read this section

Articles are grouped by the role they play in a real pipeline:

- **Foundations** (start here): production best-practices checklist; query preprocessing.
- **Architecture variants** (when vanilla RAG isn't enough): agentic-controller patterns; knowledge-graph–augmented retrieval.
- **Specific implementations** (substrates): KAG (OpenSPG / Ant Group); Neo4j GraphRAG.

### Foundations

- [[summary_of_best_practices]] — RAG production checklist: chunking, embeddings, hybrid search, reranking, eval.
- [[rag_query_summarize]] — query preprocessing: HyDE, multi-query, decomposition, step-back, RAG-Fusion.

### Architecture variants

- [[agentic_rag]] — LLM as retrieval controller: Self-RAG, CRAG, Adaptive-RAG, ReAct over a search tool.
- [[graph_rag]] — knowledge-graph–augmented retrieval; Microsoft GraphRAG and the lighter siblings (LightRAG, nano-graphrag, HippoRAG).

### Specific implementations

- [[kag]] — Ant Group's KAG on the OpenSPG engine (schema-first, logical-form-guided).
- [[neo4j_rag]] — Neo4j as both vector index and graph store; the official `neo4j-graphrag-python` package.

## Decision shortcut

| Symptom in your eval | First thing to try |
|---|---|
| Recall is bad — relevant docs not in top-k at all | Hybrid search (BM25 + dense), then [[rag_query_summarize]] |
| Precision is bad — relevant docs retrieved but answer is wrong | Cross-encoder reranker (Cohere / BGE / Jina), then citations / "say I don't know" prompting |
| Compound questions ("compare X and Y in 2020 vs 2023") fail | Query decomposition / `SubQuestionQueryEngine` ([[rag_query_summarize]]) |
| Aggregative / "global" questions fail ("what are the main themes?") | [[graph_rag]] |
| Multi-hop factual questions need entity relationships | [[graph_rag]] or [[kag]]; [[neo4j_rag]] if you already have Neo4j |
| Some queries shouldn't trigger retrieval at all | Query classifier; if it's worth a controller loop → [[agentic_rag]] (Adaptive-RAG) |
| Retrieved chunks frequently look right but contradict the answer | Self-RAG / CRAG ([[agentic_rag]]) for verification loops |
| You can write a domain ontology and need multi-hop reasoning over it | [[kag]] (schema-first) |
| Data is genuinely graph-shaped (citations, dependencies, products) | [[neo4j_rag]] |
| You don't know what's wrong | Build an eval set (RAGAS / TruLens / golden 50–200 triples) — see [[summary_of_best_practices]] |

## See also (cross-section)

- [[../README|ml]] — Machine Learning hub.
- [[../llm/README|llm]] — LLMs that consume retrieved context (models, runtimes, prompting, inspection).
- [[../agents/README|agents]] — agent frameworks; agentic RAG sits at the boundary.
- [[../memory/README|memory]] — long-running agent memory (semantic / episodic / KV); often co-deployed with RAG.
- [[../knowledge_graph/README|knowledge_graph]] — KG fundamentals; substrate for GraphRAG / KAG.
- [[../frameworks/README|frameworks]] — LangGraph, DSPy, phidata/agno used to build RAG pipelines.
- [[../data/qdrant_vector_search|qdrant_vector_search]] — Qdrant for ML / RAG embeddings.
- [[../../db/vector/README|db/vector]] — vector-DB landscape (operator angle).
- [[../../db/nosql/README|db/nosql]] — graph & document DBs (Neo4j neighbourhood).

## Keywords

`#rag` `#retrieval` `#embeddings` `#hybrid-search` `#graph-rag` `#agentic-rag` `#knowledge-graph`
