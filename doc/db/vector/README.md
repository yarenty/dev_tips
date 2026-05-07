---
title: "Vector databases"
keywords: [vector, embedding, similarity-search, rag, semantic-search, ann, pgvector]
status: reviewed
review_date: 2026/05/03
---

# Vector databases

Landing page for vector databases — storage and ANN-search engines for high-dimensional embeddings, used for RAG (retrieval-augmented generation), semantic search, recommendations, and clustering.

## When you actually need one

The honest first question is: **do you need a dedicated vector database at all?**

- **No, if you already run [[postgresql]].** Install `pgvector`. It does HNSW and IVFFlat indexes on `vector(N)` columns, scales to millions of vectors comfortably, and keeps your embeddings transactionally consistent with the rest of your data. This is the right answer for ~80% of RAG-style apps.
- **No, if your corpus fits in memory.** A NumPy array + FAISS in-process is plenty for under ~10M vectors. No database needed.
- **Yes, if you're doing serious scale** (hundreds of millions of vectors), need hybrid search (vector + structured filters + full-text + multi-tenancy), high QPS, or want a productized control plane.

If yes, the field is crowded:

| Project | Best for |
| --- | --- |
| [[chroma]] | Easiest embedded mode for local dev / Python notebooks. |
| [[qdrant]] | Production-grade Rust engine, strong filtering, gRPC, self-hosted or cloud. |
| [Weaviate](https://weaviate.io/) | Schema + GraphQL + built-in vectoriser modules. |
| [Milvus](https://milvus.io/) | Heavyweight, GPU-aware, designed for very large fleets. |
| [LanceDB](https://lancedb.com/) | Embedded, columnar, Arrow/Lance-native. |
| pgvector | Postgres-as-vector-DB. Boring, works. |
| [SingleStore](https://www.singlestore.com/) | Vector type bolted onto an HTAP engine ([[singlestore]]). |
| [Redis Stack](https://redis.io/docs/stack/) (Redis Search) | If you already run [[redis]] and want vectors in the same tier. |

## Articles

- [chroma](chroma.md) — embedded-first vector DB; the easiest way to start.
- [qdrant](qdrant.md) — Rust, production-shaped; the natural Chroma graduation.

## See also

- `ml/data/` — for embedding workflows, RAG patterns, and Qdrant from the ML angle (`ml/data/qdrant_vector_search.md`).
- `db/relational/postgresql.md` — pgvector lives there.

## Internal links

- [[chroma]]
- [[db/vector/qdrant|Qdrant]]
- [[postgresql]]
- [[redis]]
- [[singlestore]]

## Keywords

`#vector` `#embedding` `#similarity-search` `#rag` `#semantic-search` `#ann` `#pgvector` `#db`
