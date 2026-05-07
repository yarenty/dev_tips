---
title: "Chroma — embedded-first vector DB for LLM apps"
main_link: https://www.trychroma.com/
keywords: [chroma, vector, embedding, rag, llm, langchain, llamaindex, python, embedded]
status: reviewed
---

# Chroma — embedded-first vector DB for LLM apps

**Main link:** <https://www.trychroma.com/>

Repo: <https://github.com/chroma-core/chroma> · Docs: <https://docs.trychroma.com/>

## Summary

Chroma is an open-source vector database aimed squarely at the "build a RAG app this afternoon" use case. Apache 2.0, Python and JavaScript clients, in-process embedded mode (one `pip install chromadb` and you're storing embeddings on disk locally), with a hosted cloud and a server mode for production. It integrates natively with LangChain, LlamaIndex, and the major embedding providers.

## Insight

When Chroma is the right choice:

- **Local dev and notebooks.** Embedded mode means no docker-compose, no cluster, no auth setup. You can iterate on retrieval quality in a Jupyter notebook.
- **Small-to-medium RAG apps** where your corpus is up to a few million chunks and you don't need rich filtering or sub-50ms p99.
- **You already use LangChain or LlamaIndex** and want the path-of-least-resistance vector store. The integration is excellent.
- **Prototyping** before deciding whether you'll graduate to [[qdrant]] / Weaviate / pgvector for production.

When to outgrow it:

- **Scale** — once you're past ~10M vectors with serious QPS, look at [[qdrant]] or pgvector with HNSW.
- **Rich filtering** (combining vector search with structured predicates and full-text search) is more capable in Qdrant or Weaviate.
- **Multi-tenancy / RBAC / production ops** — Chroma's server mode has improved a lot but still feels lighter than the production-shaped alternatives.
- **Already on Postgres?** Just use pgvector and skip the extra moving part entirely.

The healthy mental model: Chroma is to vector databases what SQLite is to relational ones — embedded-first, easy to start with, perfectly fine until your usage outgrows the embedded-shaped sweet spot. There is no shame in starting with Chroma and migrating later.

## References / raw notes

> Chroma — the open-source embedding database. The fastest way to build Python or JavaScript LLM apps with memory.

Headline features:

- Simple: fully-typed, fully-tested, fully-documented Python/JS API.
- Integrations: LangChain (Python and JS), LlamaIndex, plus the major embedding providers (OpenAI, Cohere, HuggingFace, Sentence Transformers).
- Same API in notebook → cluster: develop locally in-process, deploy to a server.
- Feature-rich: queries, metadata filtering, density estimation, multi-modal.
- Apache 2.0, free and open source.

Canonical "Chat your data" pattern:

1. Add documents to a Chroma collection. You can pass your own embeddings, an embedding function, or let Chroma embed them.
2. Query relevant documents with natural language; Chroma returns the top-k by cosine/L2/inner-product similarity.
3. Compose those documents into the context window of an LLM (GPT, Claude, local, …) for summarisation or QA.

## Similar / related topics

- [[db/vector/qdrant|Qdrant]] — production-grade Rust alternative; the natural Chroma graduation.
- [Weaviate](https://weaviate.io/) — schema + GraphQL + built-in vectorisers.
- [LanceDB](https://lancedb.com/) — embedded vector DB, columnar Arrow/Lance format.
- pgvector on [[postgresql]] — boring and good if you already run Postgres.
- [LangChain](https://www.langchain.com/) / [LlamaIndex](https://www.llamaindex.ai/) — orchestration frameworks Chroma plugs into.

## Internal links

- [[db/vector/qdrant|Qdrant]]
- [[postgresql]]

## Keywords

`#chroma` `#vector-db` `#embedding` `#rag` `#llm` `#langchain` `#llamaindex` `#embedded` `#db`
