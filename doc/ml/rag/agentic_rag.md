---
title: Agentic RAG — LLM as retrieval controller
main_link: https://arxiv.org/abs/2310.11511
keywords: [agentic-rag, self-rag, crag, adaptive-rag, rag, agents, react, observability]
status: reviewed
---

# Agentic RAG — LLM as retrieval controller

**Main link:** <https://arxiv.org/abs/2310.11511> (Self-RAG, Asai et al. 2023)

## Summary

Agentic RAG is the umbrella term for RAG pipelines in which an LLM acts as a *controller* that decides **when** to retrieve, **what** to query, and **whether** the retrieved content is actually useful — instead of always doing one fixed retrieval step over the raw user query. The lineage runs from vanilla single-shot RAG → query rewriting / decomposition → tool-calling agents over a retrieval tool → reflective/corrective loops (Self-RAG, CRAG) and complexity-routed strategies (Adaptive-RAG). In practice it is implemented with frameworks like [[../frameworks/langgraph|LangGraph]], LlamaIndex agents, and [[../frameworks/dspy|DSPy]].

## Insight

Reach for agentic RAG when vanilla RAG fails on multi-hop questions, ambiguous queries, queries that should *not* trigger retrieval at all, or when retrieval quality is so noisy that you need an explicit verification step. Canonical patterns worth knowing:

- **ReAct over a retrieval tool** — the simplest agentic RAG; the LLM emits `search(query)` calls in a loop.
- **Self-RAG** (Asai et al. 2023) — fine-tunes the model to emit special *reflection tokens* (`[Retrieve]`, `[IsRel]`, `[IsSup]`, `[IsUse]`) that decide retrieval and grade outputs; needs a custom-trained model.
- **CRAG — Corrective RAG** (Yan et al. 2024) — a lightweight retrieval evaluator scores each chunk as correct/ambiguous/wrong and triggers web-search fallback or query rewriting; works with off-the-shelf models.
- **Adaptive-RAG** (Jeong et al. 2024) — a small classifier routes each query to *no retrieval* / *single-step* / *multi-step* based on estimated complexity, so easy questions stay cheap.

The honest cost caveat: agentic RAG can be **5–10× more expensive in tokens and latency** than vanilla RAG because every loop adds at least one LLM call (and reflection variants add several). For high-QPS production search, prefer good preprocessing (`[[rag_query_summarize]]`) + reranking before introducing an agent loop. Observability (LangSmith, Langfuse, Arize Phoenix, OpenLLMetry) becomes non-optional once loops exist — without per-step traces, debugging is hopeless.

## Similar / related topics

- **Vanilla RAG** — single embed → top-k → stuff-into-context; the baseline agentic RAG is trying to beat.
- **Query preprocessing** — HyDE, multi-query, decomposition; cheaper than full agentic loops.
- **GraphRAG** — orthogonal axis (graph-shaped retrieval) that can be wrapped in an agent loop.
- **Tool-using agents** — agentic RAG is a special case where the only tool is `search`.
- **RAG evaluation** — RAGAS, ARES, TruLens; matters even more once you have non-deterministic loops.

## Internal links

<!-- reviewed -->
- [[README]] — RAG hub
- [[rag_query_summarize]] — query preprocessing (cheaper alternative or complement)
- [[graph_rag]] — graph-shaped retrieval (composable with agentic loops)
- [[summary_of_best_practices]] — chunking / reranking / eval foundations
- [[../frameworks/langgraph|langgraph]] — canonical framework for stateful agentic RAG graphs
- [[../frameworks/dspy|dspy]] — declarative agentic-RAG programs with optimisers
- [[../agents/README|agents]] — broader agent landscape
- [[../memory/agentic_ai_memory|agentic_ai_memory]] — long-running agents need memory too
- [[../llm/inspection/leaderboards|leaderboards]] — measuring whether the loop actually helps

## Keywords

`#agentic-rag` `#self-rag` `#crag` `#adaptive-rag` `#rag` `#agents` `#react` `#observability`

## References / raw notes

- Self-RAG (Asai et al. 2023): <https://arxiv.org/abs/2310.11511> · <https://selfrag.github.io/>
- CRAG — Corrective RAG (Yan et al. 2024): <https://arxiv.org/abs/2401.15884>
- Adaptive-RAG (Jeong et al. 2024): <https://arxiv.org/abs/2403.14403>
- ReAct (Yao et al. 2022): <https://arxiv.org/abs/2210.03629>
- LangGraph agentic-RAG cookbook: <https://langchain-ai.github.io/langgraph/tutorials/rag/langgraph_agentic_rag/>
- LlamaIndex agentic strategies: <https://docs.llamaindex.ai/en/stable/optimizing/agentic_strategies/agentic_strategies/>
- Observability for RAG agents (DecodingML): <https://decodingml.substack.com/p/observability-for-rag-agents>
