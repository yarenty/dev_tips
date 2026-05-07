---
title: Query preprocessing for RAG — HyDE, multi-query, decomposition, step-back, RAG-Fusion
main_link: https://arxiv.org/abs/2212.10496
keywords: [rag, query-rewriting, hyde, multi-query, query-decomposition, step-back, rag-fusion]
status: reviewed
---

# Query preprocessing for RAG — HyDE, multi-query, decomposition, step-back, RAG-Fusion

**Main link:** <https://arxiv.org/abs/2212.10496> (Gao et al. 2022, *Precise Zero-Shot Dense Retrieval without Relevance Labels* — HyDE)

## Summary

"Query summarize" is the loose umbrella for **transforming the user's query before retrieval** to improve recall when the raw query is short, ambiguous, multi-part, or worded very differently from the corpus it's being matched against. The canonical patterns are HyDE (embed a hypothetical answer, not the question), multi-query (generate N paraphrases and union the results), query decomposition (break a complex question into sub-questions), step-back prompting (ask a more general question first), and RAG-Fusion (multi-query + Reciprocal Rank Fusion). Every variant trades **one or more extra LLM calls** for **better top-k recall**, and they all sit *upstream* of the rest of the RAG pipeline (chunking, hybrid search, reranking — see [[summary_of_best_practices]]).

## Insight

Reach for query preprocessing once you've tuned the basics (chunking + hybrid search + reranker) and your eval set still shows recall problems on a recognisable subset of queries: short keyword queries, queries that use vocabulary the corpus doesn't, or compound questions. The patterns map cleanly to symptoms:

- **HyDE** — *Hypothetical Document Embeddings* (Gao et al. 2022). Have the LLM write a plausible *answer* to the question and embed **that** instead of the question. Works because answers tend to share more vocabulary with corpus passages than questions do. Best when the user query is terse and the corpus is verbose (academic, legal, encyclopaedic). Failure mode: the LLM hallucinates a fluent but wrong-domain answer — embeddings then drift.
- **Multi-query / Multi-Query Retriever** (LangChain pattern). Generate N (3–5) paraphrases of the user query, run retrieval on each, deduplicate the union. Cheap recall lift; pairs naturally with reranking.
- **RAG-Fusion** (Adrian Raudaschl, 2023). Multi-query + **Reciprocal Rank Fusion** to merge ranked lists rather than de-dup the union. Often beats plain multi-query because RRF rewards passages that show up across multiple variants.
- **Query decomposition / sub-questions**. For "Compare the revenue growth of A and B between 2020 and 2023", split into independently-retrievable sub-queries, fetch evidence per sub-query, then synthesise. LlamaIndex's `SubQuestionQueryEngine` is the canonical implementation.
- **Step-back prompting** (Zheng et al., DeepMind 2023). Generate a more *abstract* version of the question first (e.g. "What physics principles apply to ideal gases?") to retrieve foundational context that grounds the specific answer. Helps on reasoning-heavy queries.
- **Hypothetical-Question indexing** — symmetric variant: at *index* time, generate hypothetical questions for each chunk and embed those alongside; turns the asymmetric question-vs-passage matching into question-vs-question. Cheap inference, expensive ingestion.

The unifying trade-off is **extra LLM cost vs retrieval quality**. A multi-query + RRF setup typically adds 1 small LLM call + N parallel retrievals + RRF (microseconds) per request — usually worth it. Full agentic loops are 5–10× heavier and only earn that cost on hard query distributions; see [[agentic_rag]]. The honest gotcha: every transformation is a place to drift away from the user's intent — keep the original query in the candidate pool ("multi-query *plus* original") rather than replacing it.

## Similar / related topics

- **Reranking** — orthogonal lever; query rewriting fixes recall, reranking fixes precision; combine.
- **Hybrid search (BM25 + dense)** — also a recall lever; query rewriting often gives less when hybrid is already in place.
- **Agentic RAG** — generalises query rewriting to a controller loop; see [[agentic_rag]].
- **Query routing** — LLM picks which index/tool to query, not how to rewrite the query.
- **Conversation-aware rewriting** — for multi-turn chat: condense history + latest turn into a standalone query (the "condense question" prompt).

## Internal links

- [[README]] — RAG hub
- [[summary_of_best_practices]] — chunking / hybrid / reranking / eval foundations
- [[agentic_rag]] — controller-loop generalisation
- [[graph_rag]] — different retrieval substrate; preprocessing still applies
- [[../frameworks/dspy|dspy]] — programmatically optimise these prompts instead of hand-tuning
- [[../frameworks/langgraph|langgraph]] — orchestrate multi-step query preprocessing
- [[../data/qdrant_vector_search|qdrant_vector_search]] — typical vector substrate
- [[../llm/README|llm]] — LLM hub

## Keywords

`#rag` `#query-rewriting` `#hyde` `#multi-query` `#query-decomposition` `#step-back` `#rag-fusion`

## References / raw notes

### Canonical papers

- HyDE — *Precise Zero-Shot Dense Retrieval without Relevance Labels* (Gao, Ma, Lin, Callan 2022): <https://arxiv.org/abs/2212.10496>
- Step-Back Prompting (Zheng et al., DeepMind 2023): <https://arxiv.org/abs/2310.06117>
- Query2doc (Wang et al., Microsoft 2023) — closely related to HyDE: <https://arxiv.org/abs/2303.07678>
- Adaptive-RAG — picks query strategy by complexity (Jeong et al. 2024): <https://arxiv.org/abs/2403.14403>
- RAG-Fusion blog post (Adrian Raudaschl, 2023): <https://towardsdatascience.com/forget-rag-the-future-is-rag-fusion-1147298d8ad1>

### Library implementations

- LangChain — query transformations docs: <https://python.langchain.com/docs/how_to/#query-analysis>
- LangChain `MultiQueryRetriever`: <https://python.langchain.com/docs/how_to/MultiQueryRetriever/>
- LangChain HyDE: <https://python.langchain.com/api_reference/langchain/chains/langchain.chains.hyde.base.HypotheticalDocumentEmbedder.html>
- LlamaIndex `SubQuestionQueryEngine`: <https://docs.llamaindex.ai/en/stable/examples/query_engine/sub_question_query_engine/>
- LlamaIndex query transformations: <https://docs.llamaindex.ai/en/stable/optimizing/advanced_retrieval/query_transformations/>
- DSPy — `dspy.Retrieve` + signature-driven query rewrites: <https://dspy.ai/>
