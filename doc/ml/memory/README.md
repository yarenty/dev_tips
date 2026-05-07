---
title: Memory — LLM and agent memory architectures
keywords: [memory, agentic, mem0, memgpt, letta, zep, a-mem, episodic, caching, semantic-cache]
status: reviewed
review_date: 2026/05/03
---

# Memory — LLM and agent memory architectures

How LLM-based agents and RAG pipelines extend, structure, and recall information beyond their fixed context window. The 2024-2025 wave of work covers tiered short-term/long-term stores (Mem0, Letta), graph-structured episodic memory (Zep, A-MEM, AriGraph), memory-augmented retrieval (MemoRAG, HippoRAG), context-extension via compression (EM-LLM, InfLLM, Mamba), and the *adjacent* response-cache axis (semantic caching).

## Articles

### Landscape & surveys

- [Agentic AI memory — landscape](agentic_ai_memory.md) — start here. Three-axis mental model (temporal scope / representation / operations) + the headline projects (Mem0, MemGPT/Letta, Zep, A-MEM, Cognee).
- [Eidetic-style memory for agentic AI — 2024-2025 survey](response.md) — focused answer to *"do recent papers approach eidetic memory for agents?"* with a curated reading list.
- [AI memory — May 2025 paper digest](202505_recents.md) — time-capsule snapshot of the field circa May 2025.

### Specific threads

- [Episodic memory for LLMs and agents](episodic_memory.md) — EM-LLM (10M-token effective context via Bayesian-surprise events) / MemoRAG (dual-system) / Mamba (state-space compression) / AriGraph (KG world model) / AI-native Memory.
- [Semantic caching for LLMs and AI agents](caching.md) — the *adjacent* memory-shaped optimisation: cache the answer (GPTCache / Redis LangCache / Fastly AI Accelerator), cache the prompt prefix (Anthropic / OpenAI prompt caching), cache the KV (vLLM PagedAttention).

## Picker shortcut

| If you need… | Reach for… |
|--------------|-----------|
| Production agent with cross-session continuity | **Mem0** or **Letta** (MemGPT) |
| Time-stamped facts and sessions | **Zep** (temporal KG) |
| Self-organising agent notes (research-grade) | **A-MEM** |
| Long-document QA with multi-hop reasoning | **MemoRAG** or **HippoRAG-2** |
| Effective recall over 10M+ tokens | **EM-LLM** or **InfLLM** |
| Linear-time sequence processing (throughput first) | **Mamba** / **xLSTM** |
| Cut LLM API cost on FAQ-shaped traffic | **GPTCache** / **Redis LangCache** / **Fastly AI Accelerator** |
| Cut token cost on long static system prompts | Provider **prompt caching** (Anthropic / OpenAI) |
| Personalised lifelong assistant | **AI-native Memory / Second Me** |

## Keywords

`#memory` `#agentic-memory` `#episodic-memory` `#semantic-cache` `#mem0` `#letta` `#zep` `#a-mem` `#memorag` `#em-llm` `#mamba`

## See also

- [[../agents/README|agents]] — agentic-AI section landing.
- [[../rag/README|rag]] — RAG section landing.
- [[../llm/README|llm]] — LLM section landing.
- [[../mcp/README|mcp]] — MCP section landing (skill / tool axis adjacent to memory).
- [[../knowledge_graph/README|knowledge_graph]] — KG-shaped memory cross-link.
- [[programming/rust/data/cache|rust/data/cache]] — different topic: in-process Rust caches (Moka / quick_cache).
