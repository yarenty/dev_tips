---
title: Neo4j for RAG — vector index plus graph traversal
main_link: https://neo4j.com/docs/neo4j-graphrag-python/current/
keywords: [neo4j, neo4j-graphrag, graph-rag, vector-index, cypher, python]
status: reviewed
---

# Neo4j for RAG — vector index plus graph traversal

**Main link:** <https://neo4j.com/docs/neo4j-graphrag-python/current/>

## Summary

Since version 5.11 (Aug 2023) Neo4j ships a **native vector index** that lives next to its property graph, so you can store embeddings on graph nodes and combine semantic similarity with Cypher graph traversal in a single query. Neo4j's officially-maintained `neo4j-graphrag` Python package wraps this — it provides retrievers (vector, vector+Cypher, hybrid BM25+vector, HybridCypher), an LLM-driven KG-construction pipeline (`SimpleKGPipeline`), and integrations with OpenAI / Anthropic / Cohere / Mistral / Ollama for both embeddings and generation. It is Neo4j's house implementation of the [[graph_rag|GraphRAG]] idea.

## Insight

Reach for Neo4j-as-RAG-substrate when your data is **genuinely graph-shaped** — entity relationships materially help retrieval (legal-case citations, drug interactions, product hierarchies, code dependency graphs). For flat document corpora where the graph is invented just to satisfy GraphRAG, you'll spend a lot on extraction and operations for marginal recall gains over a good hybrid pgvector / Qdrant / [[../data/qdrant_vector_search|Qdrant for ML]] setup.

The honest comparison is "one DB" vs "two DBs":

- **Neo4j (one)** — embeddings + graph in the same store; one Cypher query can `MATCH` traversals and call `db.index.vector.queryNodes(...)` together; lower operational surface; you pay the Neo4j licence/Aura cost and inherit Neo4j's vector-search performance ceiling (less tuneable than dedicated vector DBs at very large scale).
- **Qdrant/Milvus/pgvector + a separate graph store (or relations in Postgres)** (two) — best-in-class vector search, but you join across systems in application code and operate two databases; LangChain / LlamaIndex have premade adapters for this.

Practical gotchas: (a) the Neo4j vector index is HNSW with cosine/Euclidean only — fine for most RAG, not for billion-vector workloads; (b) `SimpleKGPipeline`'s LLM-driven extraction is one LLM call per chunk and as expensive as any other GraphRAG ingestion — see [[graph_rag]]; (c) Neo4j's `neo4j-graphrag-python` is the **officially supported** path; community packages like `langchain-neo4j` and `llama-index-graph-stores-neo4j` are also widely used and integrate at higher abstraction levels.

For Rust callers there's a community async driver (`robsdedude/neo4j-rust-driver`); `neo4j-labs/graph` is an unrelated Rust *graph-algorithms* crate (PageRank/triangle counting), not a Neo4j client.

## Similar / related topics

- **GraphRAG (generic)** — the architectural pattern Neo4j is one substrate for; see [[graph_rag]].
- **KAG / OpenSPG** — schema-first competitor on a different graph stack; see [[kag]].
- **Qdrant + relational store** — "two databases" alternative; see [[../data/qdrant_vector_search|qdrant_vector_search]].
- **pgvector + Apache AGE** — Postgres-native vector + graph; cheaper but rougher than Neo4j.
- **LightRAG / nano-graphrag** — file-based GraphRAG implementations; see [[graph_rag]].

## Internal links

- [[README]] — RAG hub
- [[graph_rag]] — broader GraphRAG pattern
- [[kag]] — schema-first alternative on OpenSPG
- [[summary_of_best_practices]] — chunking / hybrid-search foundations
- [[../data/qdrant_vector_search|qdrant_vector_search]] — vector-DB alternative substrate
- [[../knowledge_graph/README|knowledge_graph]] — KG fundamentals
- [[../../db/vector/README|db/vector]] — vector-DB landscape (operator angle)
- [[../../db/nosql/README|db/nosql]] — broader graph/document-DB landscape

## Keywords

`#neo4j` `#neo4j-graphrag` `#graph-rag` `#vector-index` `#cypher` `#python` `#rag`

## References / raw notes

- `neo4j-graphrag-python` repo: <https://github.com/neo4j/neo4j-graphrag-python>
- Official docs: <https://neo4j.com/docs/neo4j-graphrag-python/current/>
- Launch blog (Python package): <https://neo4j.com/blog/graphrag-python-package/>
- Knowledge-graph RAG application tutorial: <https://neo4j.com/developer-blog/knowledge-graph-rag-application/>
- Advanced RAG strategies on Neo4j: <https://neo4j.com/developer-blog/advanced-rag-strategies-neo4j/>
- Native vector index docs: <https://neo4j.com/docs/cypher-manual/current/indexes/semantic-indexes/vector-indexes/>

### Rust-side bits

- Community async driver: <https://github.com/robsdedude/neo4j-rust-driver>
- `neo4j-labs/graph` (Rust graph *algorithms*, not a Neo4j client): <https://github.com/neo4j-labs/graph>
