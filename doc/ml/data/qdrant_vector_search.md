---
title: Qdrant for ML — embeddings, RAG, semantic & image search
main_link: https://qdrant.tech/
keywords: [qdrant, vector-search, embeddings, ann, rag, semantic-search, image-similarity]
status: reviewed
---

# Qdrant for ML — embeddings, RAG, semantic & image search

**Main link:** <https://qdrant.tech/>

## Summary

This article covers Qdrant from the **ML / application** angle: turning embeddings into a working semantic-search, RAG-retrieval, recommendation, or image-similarity feature. Qdrant itself is a Rust-written vector database — see [[../../db/vector/qdrant|db/vector/qdrant]] for the operator-side framing (install / scaling / config / cluster). The differentiator on the ML side is Qdrant's first-class **payload-filtering** model: every point can carry arbitrary JSON, and filters compose into the ANN search itself rather than running as a post-filter, which matters a lot for filtered-retrieval RAG (per-tenant, per-permission, per-date-range).

## Insight

The mental model worth internalising: a vector DB is just a fast nearest-neighbour index over `(id, embedding, payload)` rows. Once you have an embedder (sentence-transformers, OpenAI text-embedding-3, BGE, Cohere embed, CLIP for images) the actual choice between Qdrant / Weaviate / Milvus / pgvector / Chroma / LanceDB is mostly about: (a) ANN algorithm (HNSW everywhere; some offer DiskANN/PQ for cost), (b) filter expressivity at index time vs query time, (c) operational shape (managed cloud, self-host single binary, embedded library, Postgres extension).

Qdrant's sweet spot for ML use cases:

- **RAG with strict filters** — multi-tenant SaaS where you must filter by `tenant_id`, document permissions, freshness window. Qdrant's pre-filtering during HNSW traversal preserves recall better than naive post-filter on highly-selective filters.
- **Hybrid search** — Qdrant 1.10+ ships sparse-vector (BM25-style) + dense-vector fusion natively, removing one external integration.
- **Image / multimodal similarity** — CLIP or SigLIP embeddings → Qdrant; payload stores `s3_uri`, exif metadata.
- **Quantisation for cost** — scalar / product / binary quantisation built in; binary quantisation can give 32× memory reduction on 1B-scale collections at modest recall cost.

Common gotchas:

- Vector dimensionality is fixed per collection. If you change embedder, you create a new collection (and re-index everything).
- `distance` metric must match the embedder's training (cosine for most sentence-transformers, dot for some OpenAI models, euclidean rarely).
- Payload indexes are not free — index only the fields you'll filter on, otherwise upsert latency grows.
- For "do I even need a vector DB?" question, consider `pgvector` first if you already run Postgres at modest scale (<10M vectors) — see the decision matrix in [[../../db/vector/README|db/vector]].

## Similar / related topics

- **Weaviate** — Go-based, schema-first, built-in vectoriser modules, GraphQL API.
- **Milvus / Zilliz** — C++/Go, horizontally-scaled cluster mode for billion-scale; heavier ops.
- **pgvector / pgvecto.rs** — Postgres extensions, the right choice when you're already on Postgres and at <10M vectors.
- **Chroma / LanceDB** — embedded / single-process for Python prototypes; LanceDB built on the Lance columnar format.
- **OpenAI / Pinecone managed services** — fully-hosted; ops-free at the cost of lock-in and per-vector pricing.
- **CLIP / SigLIP / BGE / sentence-transformers** — the embedder side; Qdrant just stores what they produce.

## Internal links

- [[../../db/vector/qdrant|db/vector/qdrant]] — operator-side: install, config, scaling, cluster
- [[../../db/vector/README|db/vector]] — broader vector-DB landscape and "do I need one?" decision matrix
- [[../rag/README|ml/rag]] — where Qdrant is the canonical retrieval backend
- [[../llm/README|ml/llm]] — embeddings come out of LLMs / encoder models
- [[../../programming/rust/data/lance_data_format|lance_data_format]] — adjacent ML-storage format

## Keywords

`#qdrant` `#vector-search` `#embeddings` `#ann` `#rag` `#semantic-search` `#image-similarity`

## References / raw notes

### Project

- Repo: <https://github.com/qdrant/qdrant>
- Docs: <https://qdrant.tech/documentation/>
- Cloud (managed, free tier): <https://cloud.qdrant.io/>
- Benchmarks: <https://qdrant.tech/benchmarks/>

### Project's own pitch

Vector Search Engine for the next generation of AI applications.

Qdrant (read: "quadrant") is a vector similarity search engine and vector database. It provides a production-ready service with a convenient API to store, search, and manage points — vectors with an additional payload. Qdrant is tailored to extended filtering support, which makes it useful for all sorts of neural-network or semantic-based matching, faceted search, and other applications.

Qdrant is written in Rust, which makes it fast and reliable even under high load. With Qdrant, embeddings or neural-network encoders can be turned into full-fledged applications for matching, searching, recommending, and much more.

Qdrant is also available as a fully-managed Qdrant Cloud (with a free tier).

### Disambiguation note

This article focuses on the **ML application angle** (embeddings, RAG, image search, semantic search). For installation, scaling, and the operator/DBA view see [[../../db/vector/qdrant|db/vector/qdrant]] (P5.S reviewed).
