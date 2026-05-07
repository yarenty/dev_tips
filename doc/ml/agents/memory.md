---
title: Agent memory — practitioner patterns (scratchpad / vector / episodic / procedural)
main_link: https://github.com/FoundationAgents/awesome-foundation-agents#memory
keywords: [agent-memory, scratchpad, vector-memory, episodic-memory, procedural-memory, mem0, letta]
status: reviewed
review_date: 2026/05/03
---

# Agent memory — practitioner patterns

**Main link:** <https://github.com/FoundationAgents/awesome-foundation-agents#memory>

## Summary

This article is the **agent-side** view of memory: the four practical patterns that show up in every production agent (scratchpad / long-term vector store / episodic event log / procedural skills), how they compose, which library implements which, and what to reach for when. It's deliberately complementary to **[[../memory/README|ml/memory]]** (P5.AC), which covers the same territory from the **architecture-research side** — that section has the headline papers (Mem0, A-MEM, Zep, MemoRAG, EM-LLM, HippoRAG-2) and the eidetic-memory survey ([[../memory/response|ml/memory/response]]). Read this for *what to put in your agent today*; read `ml/memory` for *what the design space looks like*.

## Insight

The four practical-patterns mental model:

| Memory type | What it stores | Lifetime | Implementation pattern |
|---|---|---|---|
| **Scratchpad / working memory** | Step-by-step thinking for the current task; tool-call history | Single agent run | A list in the conversation context; truncate on overflow |
| **Long-term semantic memory** | Facts the agent has learned about its user / world | Across sessions | Vector store + embedding index (Qdrant / pgvector / Chroma / LanceDB) |
| **Episodic memory** | "What happened when" — past conversations, events, outcomes | Across sessions | Time-stamped event log; often a graph (Zep) or vector store with date metadata |
| **Procedural memory** | Learned skills / how-to recipes | Long-lived | A skill registry (Anthropic Claude Skills, OpenAI Custom GPTs, code-agent macros) |

**Decision rule.** Most production "agent with memory" systems are a fixed-size scratchpad + a vector long-term store with summarisation. That covers ~80% of useful behaviour. Add episodic memory only when the agent needs temporal reasoning ("what did the user say last Tuesday?"); add procedural memory only when you're explicitly capturing reusable skills.

**The integration question.** Where does memory write happen? Three patterns:

1. **End-of-turn extraction** (Mem0's default) — after each turn, an LLM call extracts memorable facts and writes them to the store. Simple, expensive in compute.
2. **Background consolidation** (MemGPT/Letta-style) — sleep-time agents that re-organise memory asynchronously. Less per-turn cost, more architectural complexity.
3. **Implicit memory** (KV-cache / context-extension methods) — no separate store; the model just gets a longer effective context. EM-LLM, MemoRAG, RWKV state, Mamba state all fall here.

**Common gotchas.**

- **Memory growth is unbounded** unless you have an explicit eviction policy. "Forget" is a design feature, not a bug.
- **Vector memory needs metadata filters.** Pure semantic similarity will retrieve "user said the opposite a year ago" as relevant; filter by recency / source / category.
- **Privacy is the hardest part.** Per-user memory must be partitioned at the storage layer (separate collections / row-level filters), not just at the application layer.
- **Eval is hard.** Most public memory benchmarks (LongMemEval, LoCoMo, MemBench) test retrieval quality, not the harder *did this memory help the agent succeed* question.

**Library shortlist (late 2025).**

- **Mem0** (`mem0ai/mem0`) — production-popular long-term memory with end-of-turn extraction; OSS + hosted.
- **Letta** (formerly MemGPT) — OS-style tiered memory with explicit eviction.
- **Zep** — temporal knowledge-graph memory (Graphiti underneath).
- **A-MEM** — research, Zettelkasten-style notes; see `[[../memory/agentic_ai_memory|agentic_ai_memory]]`.
- **LangChain / LlamaIndex memory primitives** — built-in chat history + vector retrievers; lowest-friction starting point.

## Similar / related topics

- **`[[../memory/agentic_ai_memory|ml/memory/agentic_ai_memory]]`** (P5.AC) — the architecture-side landscape article.
- **`[[../memory/episodic_memory|ml/memory/episodic_memory]]`** (P5.AC) — EM-LLM / MemoRAG / Mamba / AriGraph.
- **`[[../memory/response|ml/memory/response]]`** (P5.AC) — the eidetic-memory 2024-2025 survey.
- **`[[../memory/caching|ml/memory/caching]]`** (P5.AC) — semantic caching, the *other* memory-shaped optimisation.
- **`[[../rag/README|rag]]`** — RAG is a memory pattern by another name.
- **OpenAI Memory feature** — ChatGPT's product-side per-user memory (Apr 2024).
- **Anthropic Claude Memory** — closed equivalent.

## Internal links

- [[README]] — agents section landing.
- [[claude]] — augmented LLM = LLM + tools + **memory**; this is the memory side.
- [[orchestration]] — orchestration frameworks usually own the scratchpad.
- [[task]] — the task-framing question that drove the original memory survey.
- [[../memory/README|ml/memory]] (P5.AC) — **architecture-side companion section** (read both).
- [[../memory/agentic_ai_memory|ml/memory/agentic_ai_memory]] (P5.AC) — landscape article.
- [[../memory/response|ml/memory/response]] (P5.AC) — eidetic-memory survey + curated reading list.
- [[../memory/episodic_memory|ml/memory/episodic_memory]] (P5.AC) — episodic-memory architectures.
- [[../memory/caching|ml/memory/caching]] (P5.AC) — semantic caching.
- [[../rag/README|rag]] (P5.AE) — retrieval is the basic memory pattern.
- [[../rag/agentic_rag|agentic_rag]] (P5.AE) — agentic-RAG patterns that overlap heavily.
- [[../data/qdrant_vector_search|qdrant]] (P5.AB) — vector store commonly used for long-term memory.

## Keywords

`#agent-memory` `#scratchpad` `#vector-memory` `#episodic-memory` `#procedural-memory` `#mem0` `#letta`

## References / raw notes

- The awesome-foundation-agents memory section (curated index): <https://github.com/FoundationAgents/awesome-foundation-agents#memory>
- *Rethinking Memory in AI: Taxonomy, Operations, Topics, and Future Directions* (Du et al. 2025): <https://arxiv.org/abs/2505.00675>

### Original raw question (preserved as the prompt that drove [[task]] and the merged-out `response.md`)

> Has anyone looked into Agentic AI / RAG and memory solutions — could eidetic memory be a great fit for Agents/RAG/LLM? Try to find examples on arXiv or PubMed; focus on memory issues and solutions, especially within the latest years (2024/2025).

The substantive answer to that question lives in **[[../memory/response|ml/memory/response]]** (the trimmed eidetic-memory survey reviewed in P5.AC). The previous `agents/response.md` was a 1400-line auto-split duplicate; it has been **deleted in this batch (P5.AF)** to avoid duplication, and any references should point to `ml/memory/response.md` instead.

### Library quick-pointers

- Mem0: <https://github.com/mem0ai/mem0>
- Letta (formerly MemGPT): <https://github.com/letta-ai/letta>
- Zep / Graphiti: <https://github.com/getzep/zep> and <https://github.com/getzep/graphiti>
- A-MEM (research): <https://arxiv.org/abs/2502.12110>
- MemoRAG: <https://github.com/qhjqhj00/MemoRAG>
- HippoRAG / HippoRAG-2: <https://github.com/OSU-NLP-Group/HippoRAG>
- LangGraph checkpointers (memory-as-state-snapshot): <https://langchain-ai.github.io/langgraph/concepts/persistence/>
