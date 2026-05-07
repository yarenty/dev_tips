---
title: RAG production best practices — chunking, embeddings, hybrid search, reranking, eval
main_link: https://www.anthropic.com/news/contextual-retrieval
keywords: [rag, best-practices, chunking, embeddings, hybrid-search, reranking, evaluation, ragas]
status: reviewed
---

# RAG production best practices — chunking, embeddings, hybrid search, reranking, eval

**Main link:** <https://www.anthropic.com/news/contextual-retrieval> (Anthropic, *Introducing Contextual Retrieval*, Sep 2024)

## Summary

A field guide to the practices that have crystallised across 2023–2025 for shipping RAG to production: classifying which queries even need retrieval, picking a chunking strategy, choosing an embedding model, layering hybrid (BM25 + dense) search, adding a reranker, and closing the loop with evaluation and grounding/citations. None of these are exotic — the value is in **getting all of them roughly right at once**, since each compounds on the others. Anthropic's *Contextual Retrieval* post (Sep 2024) is the most-cited recent reference; RAGAS, ARES, and TruLens are the canonical evaluation toolkits.

## Insight

The 80/20 of "RAG works in production" is a small, opinionated checklist:

1. **Query classification** — not every question needs retrieval. A cheap classifier (or a small LLM call) saves both latency and grounding errors when the question is conversational or already self-contained.
2. **Chunking** — start with `RecursiveCharacterTextSplitter` (~512–1024 tokens, ~10–20% overlap), then graduate to *semantic chunking* (split on embedding-similarity dips), *sentence-window* retrieval (small chunk for matching, larger window returned to LLM), or *small-to-big* / parent-document patterns. The "right" chunk size is workload-specific — measure.
3. **Embedding model** — the modern open shortlist is **BGE-M3** (BAAI, multi-vector + multilingual), **E5-Mistral / E5-large-v2** (Microsoft), **Nomic Embed**, **mxbai-embed-large** (Mixedbread), and **Jina embeddings v3**. Closed: **OpenAI `text-embedding-3-large`**, **Cohere `embed-v3` / `embed-v4`**, **Voyage `voyage-3`**. Use the [MTEB leaderboard](https://huggingface.co/spaces/mteb/leaderboard) but trust your own eval set more.
4. **Metadata** — title, section, hypothetical-questions, summaries; almost free to add and big lift on hybrid search.
5. **Hybrid search** — BM25 + dense, fused with Reciprocal Rank Fusion (RRF) or learned weights. Almost always strictly better than pure vector search; the seminal recent demonstration is Anthropic's *Contextual Retrieval* (a one-line LLM "contextualise this chunk" prefix before embedding/BM25, claimed ~49% reduction in retrieval failures combined with reranking).
6. **Reranking** — cross-encoder rerankers on the top-50 candidates: **Cohere Rerank v3**, **BGE Reranker v2**, **Jina Reranker v2**, **mxbai-rerank**; **ColBERT / ColBERTv2 / PLAID** for late-interaction. This is usually the single biggest quality lift after hybrid.
7. **Generation** — return citations / spans; constrain the model to "say I don't know" when context is thin; consider Anthropic-style `<cite>` blocks or LlamaIndex `CitationQueryEngine`.
8. **Evaluation** — **RAGAS** (faithfulness, answer relevance, context precision/recall), **ARES**, **TruLens**, **Phoenix**, **DeepEval**. A frozen "golden set" of 50–200 (query, expected-answer, expected-sources) triples beats any LLM-judged synthetic eval over time.

The honest meta-lesson: most "we tried RAG and it didn't work" reports trace to skipping step 5 (BM25 fallback for keyword/code queries), step 6 (reranker), or step 8 (no eval set, so nothing's measurable). A query-preprocessing layer ([[rag_query_summarize]]) is the next thing to add when the basics are tuned and you're still missing relevant docs.

## Similar / related topics

- **Query preprocessing** — HyDE, multi-query, decomposition; covered in [[rag_query_summarize]].
- **GraphRAG / KAG** — different retrieval *substrate* than vector + BM25; orthogonal to most of these practices.
- **Agentic RAG** — adds a controller loop on top; see [[agentic_rag]].
- **Vector DB choice** — Qdrant / Milvus / pgvector / Weaviate / LanceDB; see `[[../data/qdrant_vector_search|qdrant_vector_search]]` and `[[../../db/vector/README|db/vector]]`.
- **LLM observability** — LangSmith / Langfuse / Arize Phoenix / OpenLLMetry; closes the loop on all of the above.

## Internal links

<!-- reviewed -->
- [[README]] — RAG hub
- [[rag_query_summarize]] — query rewriting / decomposition / HyDE
- [[agentic_rag]] — when controller loops earn their cost
- [[graph_rag]] — graph-shaped retrieval substrate
- [[neo4j_rag]] — Neo4j-as-substrate variant
- [[kag]] — schema-first KAG variant
- [[../data/qdrant_vector_search|qdrant_vector_search]] — vector DB for ML / RAG
- [[../../db/vector/README|db/vector]] — vector DB landscape (operator angle)
- [[../llm/inspection/leaderboards|leaderboards]] — for picking generators
- [[../frameworks/dspy|dspy]] — programmatic prompt-and-weight optimisation for RAG pipelines

## Keywords

`#rag` `#best-practices` `#chunking` `#embeddings` `#hybrid-search` `#reranking` `#evaluation` `#ragas`

## References / raw notes

- Anthropic, *Introducing Contextual Retrieval* (Sep 2024): <https://www.anthropic.com/news/contextual-retrieval>
- "Searching for Best Practices in RAG" (Wang et al. 2024) — survey-style paper that systematically benchmarks the choices below: <https://arxiv.org/abs/2407.01219>
- MTEB leaderboard (embeddings): <https://huggingface.co/spaces/mteb/leaderboard>
- RAGAS: <https://docs.ragas.io/>
- ARES: <https://github.com/stanford-futuredata/ARES>
- TruLens: <https://www.trulens.org/>
- Cohere Rerank: <https://cohere.com/rerank>
- BGE / FlagEmbedding: <https://github.com/FlagOpen/FlagEmbedding>
- ColBERT / RAGatouille: <https://github.com/bclavie/RAGatouille>

### Original notes (curated)

The basic techniques of a RAG workflow:

- **Query classification** — decide whether retrieval is needed; route trivial / chat queries straight to the LLM.
- **Chunking** — token-level / sentence-level / semantic; typical sizes 128 / 256 / 512 / 1024 / 2048; small-to-big and sliding windows improve quality by linking chunks.
- **Embedding model** — the lever that decides retrieval ceiling; pick by MTEB + your own eval set.
- **Metadata** — titles, keywords, hypothetical questions; cheap big lift for hybrid search.
- **Vector DB selection criteria** — index types, billion-scale support, hybrid search, cloud-native ops.
- **Retrieval methods** — query rewriting, query decomposition, HyDE-style hypothetical documents.
- **Reranking** — DLM (cross-encoder) rerankers; TILDE-style token-likelihood rerankers.
