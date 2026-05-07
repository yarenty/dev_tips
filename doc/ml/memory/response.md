---
title: Eidetic-style memory for agentic AI — 2024-2025 survey
main_link: https://github.com/FoundationAgents/awesome-foundation-agents?tab=readme-ov-file#memory
keywords: [memory, eidetic, agentic, rag, llm, survey, papers]
status: reviewed
---

# Eidetic-style memory for agentic AI — 2024-2025 survey

**Main link:** <https://github.com/FoundationAgents/awesome-foundation-agents?tab=readme-ov-file#memory>

## Summary

A literature snapshot answering one specific question: *"Has the 2024-2025 wave of agentic-AI / RAG / LLM research targeted memory architectures that approach **eidetic** (highly detailed, persistent) recall?"* Short answer: **yes — but the framing is honest about the gap.** No published system claims true eidetic memory; what they do claim is dynamic, scalable, contextually rich storage-and-retrieval that *approximates* the persistence-and-detail axis of human memory while explicitly trading off precision against efficiency. This file pulls together the headline architectures (Mem0, A-MEM, Zep, MemoRAG, EM-LLM, HippoRAG-2, AriGraph) and consolidates the survey papers that map the design space.

## Insight

Three useful framings emerge from reading this literature back-to-back:

1. **"Eidetic" is the marketing aspiration; the actual axis is *write-once-recall-forever-without-degradation*.** Every system on the reading list trades that off against latency, token cost, or hallucination risk. Read each paper for *which trade-off it picked* rather than for the eidetic-memory claim.
2. **Architectures cluster into four shapes** that keep recurring: (a) **tiered short-term + long-term store** (Mem0, MemGPT, Letta), (b) **graph-structured episodic memory** (Zep, AriGraph, A-MEM Zettelkasten-style), (c) **memory-augmented retrieval** (MemoRAG dual-system, HippoRAG-2), (d) **context-window extensions via compression / KV management** (EM-LLM, InfLLM, Mamba, Recurrent Memory Transformer). Most production systems mix two of these.
3. **The honest gap vs human memory**: today's systems lack the *associative* retrieval, *adaptive forgetting*, and *meta-cognition* that human episodic memory does for free. Du et al. 2025 ("Rethinking Memory in AI") and Wu et al. 2025 ("From Human Memory to AI Memory") are the two best survey articles to read first if you only have time for two.

When this matters in practice: agent products that need cross-session continuity (Mem0/Zep/Letta sweet spot), long-document RAG with multi-hop reasoning (HippoRAG-2, MemoRAG), and 1M+-token analysis (EM-LLM, InfLLM). When it does *not* matter: most chat assistants are still well-served by a fixed context window plus vanilla RAG; do not over-engineer.

## Similar / related topics

- **Mem0** — production-ready scalable long-term memory for AI agents (Chhikara 2025).
- **MemGPT / Letta** — OS-inspired tiered memory (main memory + archival storage with paging).
- **Zep** — temporal knowledge-graph backbone for agent memory (Rasmussen et al. 2025).
- **A-MEM** — Zettelkasten-inspired self-organising agent memory (2025).
- **MemoRAG** — dual-system "global memory drives retrieval" RAG variant (Qian et al. 2024).
- **HippoRAG / HippoRAG-2** — neurobiologically-inspired non-parametric continual learning for RAG.
- **EM-LLM** — episodic memory enabling 10M-token effective context (2024).

## Internal links

<!-- reviewed -->
- [[agentic_ai_memory]] — parent landscape article (memory operations + representation taxonomy).
- [[episodic_memory]] — sibling on EM-LLM / MemoRAG / Mamba / AriGraph specifically.
- [[202505_recents]] — sibling chronological digest of May 2025 memory papers.
- [[caching]] — semantic-caching is the *other* memory-shaped optimisation in the agentic stack.
- [[../rag/graph_rag|graph_rag]] — the graph-RAG family that overlaps with knowledge-graph-backed memory.
- [[../rag/README|rag]] — RAG section landing.
- [[../agents/README|agents]] — agentic-AI section landing.
- [[../knowledge_graph/papers|kg/papers]] — KG-side reading list (relevant for graph-shaped memory).

## Keywords

`#memory` `#eidetic` `#agentic` `#rag` `#llm` `#survey` `#papers`

## References / raw notes

### The substantive 2024-2025 prose answer (condensed)

Multiple recent studies have explored innovative memory solutions for Agentic AI, RAG systems, and LLMs that incorporate dynamic architectures and retrieval methods designed to emulate aspects of human eidetic memory.

One family of works introduces memory systems that **separate transient short-term memory from a long-term knowledge repository**, where essential information is consolidated and dynamically updated in a manner reminiscent of eidetic recall (Mem0; Cheng et al. 2024). Other research explicitly highlights the goal of approaching human-like memory precision by **combining structured representations and continuous memory refinement** in agents that utilise RAG frameworks and adaptive memory retrieval.

Research focusing on Agentic AI has detailed methods that integrate both **parametric and external memory representations** while employing operations such as addition, updating, and selective retrieval. Such structured approaches create hierarchies that support the retention of detailed episodic experiences and factual observations (Du et al. 2025 — *Rethinking Memory in AI*). Complementary studies describe techniques that extend the short-context capabilities of LLMs through **efficient memory compression and retrieval strategies**.

A separate thread targets memory efficiency vs computational cost. Some models achieve significant reductions in latency and token usage by leveraging **graph-based and hierarchical memory representations** (Liu et al. 2025 — *Advances and Challenges in Foundation Agents*). Other contributions emphasize **layered memory approaches** — encompassing sensory, short-term, and long-term components — to consolidate and retrieve information reliably across extended interactions.

The **A-MEM** model represents a noteworthy 2025 advance: it implements an agentic memory system characterised by dynamically structuring and evolving memories as interconnected notes (Zettelkasten-style). Each note stores detailed interaction data with enriched attributes such as keywords, contextual descriptions, and LLM-generated tags. A-MEM uses semantic embeddings to retrieve relevant existing notes, and updates existing notes dynamically upon integration of new relevant memories — moving toward eidetic-like richness in interconnected, contextually-informed, continuously-updated representations.

Broader surveys underscore that memory systems are increasingly designed not only to store and manage data but to **refine and update internal knowledge representations continually**, driven by the need to overcome fixed context-window limitations and to endow agents with eidetic-like properties.

### Curated reading list (the strongest 2024-2025 papers)

**Survey / taxonomy (read these first):**

- Du et al. 2025 — *Rethinking Memory in AI: Taxonomy, Operations, Topics, and Future Directions.* arXiv [2505.00675](https://arxiv.org/abs/2505.00675). The cleanest taxonomy of memory operations (consolidation / updating / indexing / forgetting / retrieval / compression) and representations (parametric / contextual-unstructured / contextual-structured).
- Wu, Zhang, Wang 2025 — *From Human Memory to AI Memory: A Survey on Memory Mechanisms in the Era of LLMs.* arXiv 2025.
- Liu et al. 2025 — *Advances and Challenges in Foundation Agents: From Brain-Inspired Intelligence to Evolutionary, Collaborative, and Safe Systems.* arXiv [2504.01990](https://arxiv.org/abs/2504.01990). The foundational-agent survey; ~200 pages.
- Cheng et al. 2024 — *Exploring Large Language Model Based Intelligent Agents.* arXiv [2401.03428](https://arxiv.org/abs/2401.03428).
- Mumuni & Mumuni 2025 — *Large Language Models for AGI: A Survey of Foundational Principles.* arXiv [2501.03151](https://arxiv.org/abs/2501.03151).
- Feng et al. 2024 — *How Far Are We From AGI: Are LLMs All We Need?* arXiv 2024.

**Agentic-memory systems (the headline architectures):**

- Chhikara et al. 2025 — *Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory.* arXiv [2504.19413](https://arxiv.org/abs/2504.19413).
- Xu et al. 2025 — *A-MEM: Agentic Memory for LLM Agents* (Zettelkasten-inspired). arXiv [2502.12110](https://arxiv.org/abs/2502.12110).
- Rasmussen et al. 2025 — *Zep: A Temporal Knowledge Graph Architecture for Agent Memory.* arXiv 2025.
- Anokhin et al. 2024 — *AriGraph: Learning Knowledge Graph World Models with Episodic Memory for LLM Agents.* arXiv [2407.04363](https://arxiv.org/abs/2407.04363).
- Pink et al. 2025 — *Position: Episodic Memory is the Missing Piece for Long-Term LLM Agents.* arXiv 2025.
- DeChant 2025 — *Episodic memory in AI agents poses risks that should be studied and mitigated.*
- Helmi 2025 — *Decentralizing AI Memory: SHIMI, a Semantic Hierarchical Memory Index for Scalable Agent Reasoning.*
- Zheng et al. 2025 — *Lifelong Learning of Large Language Model based Agents: A Roadmap.*

**RAG-side memory:**

- Qian et al. 2024 — *MemoRAG: Boosting Long Context Processing with Global Memory-Enhanced Retrieval Augmentation.* arXiv [2409.05591](https://arxiv.org/abs/2409.05591).
- *From RAG to Memory: Non-Parametric Continual Learning for LLMs* (HippoRAG-2). arXiv [2502.14802](https://arxiv.org/abs/2502.14802).
- Singh et al. 2025 — *Agentic Retrieval-Augmented Generation: A Survey on Agentic RAG.* arXiv [2501.09136](https://arxiv.org/abs/2501.09136).
- Cheng et al. 2025 — *A Survey on Knowledge-Oriented Retrieval-Augmented Generation.*
- Zhang et al. 2025 — *A Survey of Graph Retrieval-Augmented Generation for Customised LLMs.*
- Peng et al. 2024 — *Graph Retrieval-Augmented Generation: A Survey.*
- Abootorabi et al. 2025 — *Ask in Any Modality: Multimodal RAG Survey.*

**Long-context / efficient-memory mechanisms:**

- *EM-LLM: Human-Like Episodic Memory for Infinite Contexts.* [arXiv 2407.09450](https://arxiv.org/abs/2407.09450), [project page](https://em-llm.github.io/). Effective retrieval at 10M tokens.
- *InfLLM: Training-Free Long-Context Extrapolation for LLMs with an Efficient Context Memory.* NeurIPS 2024.
- Schmied et al. 2024 — *Retrieval-Augmented Decision Transformer: External Memory for In-context RL.*
- Albert Gu — Mamba (state-space working-memory compression).
- Liu et al. 2025 — *A Comprehensive Survey on Long Context Language Modeling.*

**Agentic-AI broader context (memory is a sub-topic):**

- Plaat et al. 2025 — *Agentic Large Language Models, a survey.*
- Luo et al. 2025 — *Large Language Model Agent: A Survey on Methodology, Applications and Challenges.*
- Bousetouane 2025 — *Agentic Systems: A Guide to Transforming Industries with Vertical AI Agents.*
- Krishnan 2025 — *AI Agents: Evolution, Architecture, and Real-World Applications.*
- Du et al. 2025 — *A Survey on the Optimization of Large Language Model-based Agents.*
- Yang et al. 2024 — *Multi-LLM-Agent Systems: Techniques and Business Perspectives.*

### See also

- Curated landing: <https://github.com/FoundationAgents/awesome-foundation-agents> (the `#memory` section is the live-updated index this article was built from).
- Cognitive-psychology background on eidetic memory: BetterUp [overview](https://www.betterup.com/blog/eidetic-memory) — useful framing for the gap between the marketing term and the actual phenomenon.
