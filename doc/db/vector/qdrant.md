---
title: "Qdrant — production-grade Rust vector database"
main_link: https://qdrant.tech/
keywords: [qdrant, vector, embedding, similarity-search, rust, ann, hnsw, rag]
status: reviewed
---

# Qdrant — production-grade Rust vector database

**Main link:** <https://qdrant.tech/>

Repo: <https://github.com/qdrant/qdrant> · Docs: <https://qdrant.tech/documentation/> · Benchmarks: <https://qdrant.tech/benchmarks/> · Cloud: <https://cloud.qdrant.io/>

## Summary

Qdrant (pronounced "quadrant") is a vector similarity search engine and vector database written in Rust. It stores **points** — vectors with an arbitrary JSON payload — and provides fast ANN search (HNSW under the hood) plus rich, predicate-style filtering on the payload, exposed over both REST and gRPC. It's open source (Apache 2.0), self-hosted or available as Qdrant Cloud (with a free tier), and is widely considered the production-grade choice in the open-source vector-DB space.

> Note: there's a sibling article in this vault at `ml/data/qdrant_vector_search.md` that covers Qdrant from the ML / RAG application angle. This article focuses on Qdrant as a database. The two will be unified or cross-linked properly in P5.AA.

## Insight

Qdrant's strengths, vs the field:

- **Real filtering.** Most vector DBs treat metadata as an afterthought; Qdrant treats it as first-class. You can combine vector search with `must` / `should` / `must_not` predicates over arbitrary payload fields, with proper indexing on those fields. This matters a lot for multi-tenant RAG ("only my docs"), recommendation ("only items I haven't bought"), and compliance ("only documents I'm allowed to see").
- **gRPC + REST.** Both are first-class, both fast.
- **Quantisation** (scalar, product, binary) for memory-efficient storage of large collections.
- **Sharding and replication** built in; no Kubernetes operator required for a small cluster.
- **Rust** means single-binary deployment and predictable resource usage. The benchmark numbers vs Weaviate / Milvus / Elasticsearch are consistently good.

When to pick Qdrant vs the alternatives:

- **vs [[chroma]]** — Chroma is the easier on-ramp; Qdrant is the natural production graduation. Qdrant's payload filtering is significantly more powerful.
- **vs [Weaviate](https://weaviate.io/)** — Weaviate has built-in vectoriser modules and a GraphQL API; Qdrant is leaner and has better filtering. Pick Weaviate if you want the schema + GraphQL story; pick Qdrant if you want a leaner engine you assemble around.
- **vs [Milvus](https://milvus.io/)** — Milvus is heavier, GPU-aware, designed for very large fleets. For most apps, Qdrant is enough and easier to run.
- **vs pgvector** — if you already run Postgres at meaningful scale and only need a few million vectors with simple filtering, just use pgvector. Qdrant pulls ahead at higher scale, with rich filtering needs, or where you want vectors decoupled from your operational DB.

The free tier on Qdrant Cloud (a small managed cluster) is genuinely useful for prototyping and small-scale prod.

## References / raw notes

> Qdrant (read: quadrant) is a vector similarity search engine and vector database. It provides a production-ready service with a convenient API to store, search, and manage points — vectors with an additional payload. Qdrant is tailored to extended filtering support. It makes it useful for all sorts of neural-network or semantic-based matching, faceted search, and other applications.
>
> Qdrant is written in Rust 🦀, which makes it fast and reliable even under high load.

Useful links:

- Repo: <https://github.com/qdrant/qdrant>
- Benchmarks (with methodology): <https://qdrant.tech/benchmarks/>
- Qdrant Cloud (managed, free tier): <https://cloud.qdrant.io/>

## Similar / related topics

- [[chroma]] — the lighter on-ramp; embedded mode for prototyping.
- [Weaviate](https://weaviate.io/) — alternative open-source vector DB with built-in vectorisers + GraphQL.
- [Milvus](https://milvus.io/) — heavier, GPU-friendly alternative.
- pgvector on [[postgresql]] — when "just use Postgres" is enough.
- [[singlestore]] — vector type bolted onto an HTAP engine, useful if you want it next to operational data.

## Internal links

- [[chroma]]
- [[postgresql]]
- [[singlestore]]

## Keywords

`#qdrant` `#vector-db` `#similarity-search` `#rust` `#hnsw` `#rag` `#ann` `#embedding` `#db`
