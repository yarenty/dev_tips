---
title: Agentic AI memory — landscape
main_link: https://github.com/FoundationAgents/awesome-foundation-agents?tab=readme-ov-file#memory
keywords: [agentic-ai-memory, memory, mem0, memgpt, letta, zep, a-mem, taxonomy]
status: reviewed
review_date: 2026/05/03
---

# Agentic AI memory — landscape

**Main link:** <https://github.com/FoundationAgents/awesome-foundation-agents?tab=readme-ov-file#memory>

## Summary

The parent landing for the memory subsection: a map of *what an agentic-AI memory system actually consists of*, the canonical projects shipping it today (Mem0, MemGPT/Letta, Zep, A-MEM, Cognee), and the operations / representation taxonomy that lets you compare them apples-to-apples. Use this article first to get oriented; the sibling articles drill into specific angles (eidetic-style approximations, episodic memory, semantic caching, the May 2025 paper digest).

## Insight

The useful mental model has **three axes**:

1. **Temporal scope** — short-term (in-context, working memory) vs long-term (cross-session, persistent). Most chat assistants only have the former; "agentic memory" is mostly about adding the latter without breaking the former.
2. **Representation** — *parametric* (baked into model weights via fine-tuning / knowledge editing), *contextual unstructured* (text snippets in a vector store), *contextual structured* (knowledge graph, relational table, ontology). Production systems pick 1-2; the dream is a clean hand-off between them.
3. **Operations** — Du et al. 2025's six atomic operations: **consolidation** (short→long), **updating** (modify in place), **indexing** (build access paths), **forgetting** (delete / down-weight), **retrieval** (read), **compression** (squeeze). Most systems do 3-4 of these well and ignore the rest.

When to reach for a dedicated memory layer (vs just bigger context windows): **whenever your agent needs cross-session continuity** (user preferences, project history, conversation continuity beyond one chat), or when retrieval needs to be cheaper / more semantically structured than dumping the entire history into the prompt. When *not* to: most one-shot tasks, most stateless RAG, and most cases where a simple `summary` field in your conversation store would do the job.

The headline 2024-2025 projects:

- **Mem0** — production-shaped memory layer, scalable long-term store with extract / update / forget cycle. The current default pick for most products.
- **MemGPT / Letta** — OS-inspired tiered memory (main memory + archival); Letta is the rebranded sponsor company.
- **Zep** — temporal knowledge-graph backbone; sessions + facts + episodes.
- **A-MEM** — Zettelkasten-inspired self-organising notes; agent autonomously links and refines.
- **Cognee** — graph-shaped memory with ECL (extract / cognify / load) pipeline.

## Similar / related topics

- **Mem0** — production-ready scalable long-term memory layer for AI agents.
- **MemGPT / Letta** — OS-inspired tiered memory with main/archival paging.
- **Zep** — temporal knowledge graph for agent memory (sessions + facts + episodes).
- **A-MEM** — Zettelkasten-style self-organising agentic memory.
- **Cognee** — graph-based ECL (extract/cognify/load) memory layer.

## Internal links

- [[response]] — sibling: 2024-2025 eidetic-memory survey + curated reading list.
- [[episodic_memory]] — sibling: EM-LLM / MemoRAG / Mamba / AriGraph (episodic-memory thread).
- [[202505_recents]] — sibling: chronological digest of May 2025 memory papers.
- [[caching]] — semantic-caching is the *adjacent* memory-shaped optimisation (cache the answer, not the conversation).
- [[../agents/memory|agents/memory]] — agent-side framing of the same topic (cross-section sibling).
- [[../rag/README|rag]] — RAG section landing (memory-augmented RAG lives there too).
- [[../agents/README|agents]] — agentic-AI section landing.
- [[../knowledge_graph/papers|kg/papers]] — knowledge-graph reading list (graph-shaped memory).

## Keywords

`#agentic-ai-memory` `#memory` `#mem0` `#memgpt` `#letta` `#zep` `#a-mem` `#taxonomy`

## References / raw notes

### Original prompting question (preserved for context)

> *"Has anyone looked into Agentic AI / RAG and memory solutions — could eidetic memory be a great solution for Agents/RAG/LLM? Could you find examples (arXiv, PubMed)? Focus on memory issues / solutions, especially in 2024-2025."*

The detailed survey answer to this question is in the sibling [[response]] article. This page is the broader landscape orientation; the sibling is the targeted answer.

### Headline projects — homepages

- **Mem0** — <https://mem0.ai> | <https://github.com/mem0ai/mem0>
- **Letta (formerly MemGPT)** — <https://letta.com> | <https://github.com/letta-ai/letta>
- **Zep** — <https://www.getzep.com> | <https://github.com/getzep/zep>
- **A-MEM** — <https://github.com/agiresearch/A-mem> ([arXiv 2502.12110](https://arxiv.org/abs/2502.12110))
- **Cognee** — <https://www.cognee.ai> | <https://github.com/topoteretes/cognee>

### Best survey / taxonomy papers (2025)

- Du et al. 2025 — *Rethinking Memory in AI: Taxonomy, Operations, Topics, and Future Directions.* [arXiv 2505.00675](https://arxiv.org/abs/2505.00675). The cleanest taxonomy.
- Liu et al. 2025 — *Advances and Challenges in Foundation Agents.* [arXiv 2504.01990](https://arxiv.org/abs/2504.01990).
- Wu, Zhang, Wang 2025 — *From Human Memory to AI Memory: A Survey on Memory Mechanisms in the Era of LLMs.*

### Curated reading-list source

- <https://github.com/FoundationAgents/awesome-foundation-agents?tab=readme-ov-file#memory> — the live curated `#memory` section of the awesome-foundation-agents list.
