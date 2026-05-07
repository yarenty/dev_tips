---
title: Published — paper summaries and external write-ups
keywords: [published, papers, summaries, blog]
status: reviewed
review_date: 2026/05/03
---

# Published — paper summaries and external write-ups

This section is a small parking spot for **paper summaries and external write-ups** that don't fit cleanly into one of the topic sections.

The historical name comes from blog posts originally published on `yarenty.blogspot.com`, but in practice the current contents are **distilled summaries of recent ML/agent papers** — the kind of dense "abstract + methodology + results + limitations" digests you'd produce after a careful read.

If you're adding a *new* paper summary that fits a sharper topic (e.g. a RAG paper, a time-series paper, a fine-tuning paper), prefer adding it to that section's `papers.md` reading list instead. This section is for cross-cutting / agent-architecture / data-quality work that doesn't have a natural home.

## Articles

- [[data_interpreter|Data Interpreter — LLM agent for end-to-end data science]] — Hong et al. (DeepWisdom / MetaGPT, Feb 2024). The hierarchical-graph + programmable-node-generation pattern that became the canonical reference for agentic data-science architecture.
- [[dataman|DataMan — LLM-driven pre-training data selection]] — Peng et al. (Alibaba Tongyi, Feb 2025). Reverse-thinking with 14 LLM-self-identified quality criteria + 15 domain types beats DSIR, perplexity filtering, and Qurating on perplexity / ICL / instruction-following.

## See also

- [[../ml/agents/coding_agents|coding_agents]] — broader landscape of agentic-coding tools (Data Interpreter sits in the data-science niche of this).
- [[../ml/agents/co_scientist|co_scientist]] — Google's multi-agent scientific reasoning system (architecturally adjacent to Data Interpreter).
- [[../ml/data/public|public datasets]] — the broader pre-training-corpora landscape (DataMan curates from these).
- [[../db/quality/data_quality|data_quality]] — the data-quality-as-a-discipline angle (DataMan operationalises some of these dimensions for LLM training).
- [[../ml/llm/inspection/leaderboards|leaderboards]] — where these papers' downstream wins (or losses) get measured.
- [[../ml/time_series/papers|time_series/papers]] — the time-series-specific reading list, an example of where future paper summaries should go if they fit.
- [[../ml/knowledge_graph/papers|knowledge_graph/papers]] — the KG-specific reading list.
- [[../db/streaming/streaming_query_approximation/papers|streaming/papers]] — the AQP/streaming-specific reading list.

## Keywords

`#published` `#papers` `#summaries` `#paper-notes`
