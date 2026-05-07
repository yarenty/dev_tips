---
title: AI memory — May 2025 paper digest
main_link: https://github.com/FoundationAgents/awesome-foundation-agents
keywords: [202505, memory, papers, digest, agentic, rag, llm, time-capsule]
status: reviewed
---

# AI memory — May 2025 paper digest

**Main link:** <https://github.com/FoundationAgents/awesome-foundation-agents>

## Summary

A *snapshot* of the AI-memory literature as it stood in May 2025: condensed survey-of-surveys covering memory limitations in LLMs, the emerging architectures (A-MEM, MemoRAG, EM-LLM, AI-native Memory), the eidetic-memory aspiration, agentic-AI memory needs (multi-agent, planning), RAG-specific memory mechanisms, and cross-cutting trends (long-context, dual-system, cognitive-psychology inspiration). Treat this as a **time-capsule reading list** — the field is moving fast and individual claims here will go stale; the architecture *categories* and the *evaluation gaps* are the durable part.

## Insight

Useful to read this article as a **snapshot**, not a guide. Three caveats:

1. **It will date quickly.** Mem0/Letta/Zep ship breaking releases monthly; the LLM context-window numbers (32K/128K headline) are already obsolete (1M+ Gemini and Claude 4 by mid-2025). Use this as a 2024-Q1-2025 baseline; check the headline projects' current docs for live numbers.
2. **The *categories* are stable**: agentic memory systems (A-MEM-shaped), dual-system RAG (MemoRAG-shaped), context-extension via compression (EM-LLM, InfLLM, Mamba), parametric/AI-native memory (Second Me). Future work will fill these buckets, not invent new ones — at least not for a while.
3. **The *evaluation* problem is real**: LongBench, InfiniteBench, CORAL, BABILong are all still racing to capture what "good agent memory" actually means. No benchmark agreed-upon-by-all yet; treat the headline performance numbers as marketing.

When to come back to this: when reviewing a memory-research direction and wanting a 2025-Q1 baseline of what was claimed where; when building a literature-review section and wanting a citation manifest from this period; when comparing a current 2026+ paper to "the state of the art at survey time."

For an *up-to-date* version of the same list, prefer:

- The live `#memory` section of <https://github.com/FoundationAgents/awesome-foundation-agents> (this article's source).
- The sibling [[response]] (a more focused survey on the eidetic-memory question).
- The sibling [[episodic_memory]] (deeper dive on the EM-LLM / MemoRAG / Mamba / AriGraph thread).

## Similar / related topics

- [Awesome-LLM-Long-Context-Modeling](https://github.com/Xnhyacinth/Awesome-LLM-Long-Context-Modeling) — live counterpart focused on long-context.
- [Efficient-LLMs-Survey](https://github.com/AIoT-MLSys-Lab/Efficient-LLMs-Survey) — TMLR 2024 survey + repo.
- *FoundationAgents* — the live awesome-list this digest was extracted from.
- *Du et al. 2025* (*Rethinking Memory in AI*) — the cleanest taxonomy paper to anchor this digest against.

## Internal links

<!-- reviewed -->
- [[agentic_ai_memory]] — parent landscape article (memory operations + representation taxonomy).
- [[response]] — sibling: focused 2024-2025 eidetic-memory survey.
- [[episodic_memory]] — sibling: episodic-memory thread (EM-LLM / MemoRAG / Mamba / AriGraph).
- [[caching]] — sibling: semantic-caching as the adjacent memory-shaped optimisation.
- [[../rag/README|rag]] — RAG section landing.
- [[../agents/README|agents]] — agentic-AI section landing.
- [[../knowledge_graph/papers|kg/papers]] — knowledge-graph reading list.

## Keywords

`#202505` `#memory` `#papers` `#digest` `#agentic` `#rag` `#llm` `#time-capsule`

## References / raw notes

### Original digest (preserved as a 2025-Q2 snapshot)

#### Why memory matters

Memory is foundational for advanced AI — particularly LLM-based and agentic / RAG systems — to sustain coherent dialogue, prolonged interactions, and intricate tasks. Storage / retrieval / memory-grounded generation are the three durable axes; effective memory is what moves LLMs from pattern recognition toward context-aware processing. The aspirational benchmark is **eidetic memory** — perfect detailed recall — but no current system claims to have achieved it; user-facing examples like Microsoft's *Recall* (Copilot+ PCs, 2024) reflect the demand for highly-detailed persistent memory even when the implementation falls short of the eidetic ideal.

#### Current limitations

- **Fixed-length context window** restricts how much can be processed in one pass. As of May 2025 production frontier was 32K-128K-1M depending on provider; longer is increasingly available but with attention-degradation and "lost in the middle" failure modes.
- **Statelessness**: LLMs don't remember past conversations unless the history is replayed in the prompt. No built-in long-term consolidation analogous to the hippocampus → neocortex pipeline in human memory.
- **Catastrophic forgetting** during continual learning. RAG side-steps this by leaving the model fixed and adding an external knowledge layer.
- **Agentic-specific** memory needs (action plans, environment state, sequential reasoning) go beyond conversational context.
- **RAG-specific** bottlenecks: retrieval relevance, multi-hop reasoning, integrating retrieved knowledge into generation coherently.

#### 2024-2025 architectural advances

- **Agentic memory systems** with dynamic structuring + organic evolution. Headline: **A-MEM** (Zettelkasten-inspired interconnected note network with autonomous descriptions and dynamic linking).
- **Six-operation taxonomy** (Du et al. 2025): consolidation, updating, indexing, forgetting, retrieval, compression — a granular vocabulary for comparing systems.
- **Three representation types**: parametric (in-weights, fast/opaque), contextual unstructured (multi-modal vector store, flexible/grounded), contextual structured (KG / relational, queryable/precise).
- **Eidetic-inspired persistent recall**: Microsoft Recall as the user-facing aspiration; replicating true eidetic memory remains a long-term challenge.
- **Memory for agents**: experiential learning (ExpeL), workflow memory (Agent Workflow Memory), shared memory across multi-agent systems.
- **Memory-augmented RAG**: **MemoRAG** dual-system architecture (light-but-long-range memory generates draft answers → guides retrieval → expressive LLM produces final response). **HippoRAG / HippoRAG-2** for non-parametric continual learning. Structure-augmented RAG using KGs.
- **Long-context for LLMs**: **InfLLM** training-free context extension; KV-cache management (drop / store / select); context retrieval and compression. Project **AI-native Memory 2.0 / Second Me** for personalised parametric memory.

#### Cross-cutting themes

- Long-context processing as a fundamental challenge.
- Agentic principles for memory management (autonomous, adaptive).
- Multi-modal memory (text + image + audio + video).
- Dual-system architectures separating memory from core LLM processing.
- Cognitive-psychology inspiration (short/long-term, episodic/semantic/procedural).
- Evaluation: LongBench, InfiniteBench, CORAL, BABILong — no consensus benchmark yet.

### Cited works (May 2025 snapshot)

> The numbered citations below are preserved as the May 2025 link manifest the original digest assembled. Many newer papers exist now; see the headline awesome-list for the live version.

1. *Rethinking Memory in AI: Taxonomy, Operations, Topics, and Future Directions.* [arXiv 2505.00675](https://arxiv.org/abs/2505.00675).
2. *Procedural Memory Is Not All You Need.* [arXiv 2505.03434](https://arxiv.org/abs/2505.03434).
3. *From RAG to Memory: Non-Parametric Continual Learning for LLMs* (HippoRAG-2). [arXiv 2502.14802](https://arxiv.org/abs/2502.14802).
4. *A-MEM: Agentic Memory for LLM Agents.* [arXiv 2502.12110](https://arxiv.org/abs/2502.12110). Project: <https://github.com/agiresearch/A-mem>.
5. *MemoRAG: Boosting Long Context Processing with Global Memory-Enhanced Retrieval Augmentation.* [arXiv 2409.05591](https://arxiv.org/abs/2409.05591).
6. *Agentic Retrieval-Augmented Generation: A Survey on Agentic RAG.* [arXiv 2501.09136](https://arxiv.org/abs/2501.09136).
7. *Agentic Large Language Models, a survey.* [arXiv 2503.23037](https://arxiv.org/abs/2503.23037).
8. *Multi-Agent Collaboration Mechanisms: A Survey of LLMs.* [arXiv 2501.06322](https://arxiv.org/abs/2501.06322).
9. *AI-native Memory 2.0 / Second Me.* arXiv 2503.08102.
10. *Retrieval Augmented Generation and Understanding in Vision: A Survey and New Outlook.* [arXiv 2503.18016](https://arxiv.org/abs/2503.18016).
11. *Comprehensive Survey on Long Context Language Modeling.* arXiv 2025.
12. *NeurIPS 2024 — InfLLM: Training-Free Long-Context Extrapolation for LLMs with an Efficient Context Memory.*
13. *NeurIPS 2024 — BABILong: Testing the Limits of LLMs with Long Context Reasoning-in-a-Haystack.*
14. *Efficient LLMs Survey* (TMLR 2024) — <https://github.com/AIoT-MLSys-Lab/Efficient-LLMs-Survey>.
15. *Awesome-LLM-Long-Context-Modeling* — <https://github.com/Xnhyacinth/Awesome-LLM-Long-Context-Modeling>.
16. *Awesome Foundation Agents* — <https://github.com/FoundationAgents/awesome-foundation-agents> (the live curated list this digest was distilled from).
17. *Microsoft Recall* (Copilot+ PCs) — see ZDNet / Information Age coverage on the controversy and 2025 re-release.
18. *Eidetic memory* primer — <https://www.betterup.com/blog/eidetic-memory>.
