---
title: Episodic memory for LLMs and agents
main_link: https://em-llm.github.io/
keywords: [episodic-memory, em-llm, memorag, mamba, arigraph, ai-native-memory]
status: reviewed
review_date: 2026/05/03
---

# Episodic memory for LLMs and agents

**Main link:** <https://em-llm.github.io/>

## Summary

Episodic memory in cognitive psychology is the memory of *specific events* — what happened, when, where, with whom — as opposed to semantic memory (general facts) or procedural memory (skills). Recent (2024-2025) AI work tries to give LLM-based agents an analogous capability: store the agent's stream of past interactions/events in a way that can be retrieved by *content + time + context*, not just by vector similarity. The headline systems collected here (EM-LLM, MemoRAG, AriGraph, Mamba, AI-native Memory / Second Me) each pick a different mechanism — surprise-based event segmentation, dual-system retrieval, knowledge-graph world models, state-space compression, deep-network compression — to approach the same goal.

## Insight

The unifying claim across these papers: **a fixed context window is not enough; episodic recall needs an explicit storage layer with time + event structure.** What's interesting is *how* each system slices the problem:

- **EM-LLM** segments the token stream into "events" using **Bayesian surprise** + graph-theoretic refinement, then does two-stage retrieval (similarity + temporal) — claims effective retrieval at 10M tokens without retraining.
- **MemoRAG** is dual-system: a small long-range LLM forms a *global memory* of the corpus, generates draft answers as retrieval queries, and a stronger LLM produces the final answer. The "memory" guides retrieval rather than being retrieved directly.
- **AriGraph** builds an evolving **knowledge-graph world model** from agent interactions, integrating semantic + episodic edges; the agent reasons over this graph to plan.
- **Mamba** (Albert Gu) is a *state-space* alternative to attention — compresses prior tokens into a fixed-size summary, trading detailed recall for linear-time scaling. Falcon-Mamba 7B is the canonical open weights.
- **AI-native Memory / Second Me** push the idea of *parametric* episodic memory — a small personal model that learns from your interaction history and serves as a compressed lifelong memory.

When to pick which: EM-LLM if you have one giant document/transcript and need precise long-range recall. MemoRAG if you have a corpus and ambiguous queries. AriGraph if your agent operates in a structured environment with entities and relations. Mamba if you care about throughput more than perfect recall. AI-native if you're building a personalised assistant.

The honest gap: **none of these match the *associative + adaptive-forgetting* qualities of human episodic memory.** Pink et al. 2025 (*Episodic memory is the missing piece for long-term LLM agents*) explicitly frames this as the open research direction.

## Similar / related topics

- **EM-LLM** — surprise-segmented events + two-stage retrieval; 10M-token effective context.
- **MemoRAG** — dual-system "global memory drives retrieval" RAG variant.
- **AriGraph** — knowledge-graph world model with semantic + episodic edges.
- **Mamba** — state-space working-memory compression alternative to attention.
- **AI-native Memory / Second Me** — parametric lifelong personal memory via a small dedicated model.
- **MemGPT / Letta** — OS-style tiered memory (main + archival) with paging.

## Internal links

- [[agentic_ai_memory]] — parent landscape article (memory operations + representation taxonomy).
- [[response]] — sibling: 2024-2025 eidetic-memory survey (broader citation list).
- [[202505_recents]] — sibling: chronological digest of May 2025 memory papers.
- [[caching]] — adjacent optimisation (semantic caching of LLM answers).
- [[../knowledge_graph/papers|kg/papers]] — KG-side reading list (relevant for AriGraph-style graph memory).
- [[../rag/graph_rag|graph_rag]] — graph-RAG family overlaps with KG-shaped episodic memory.
- [[../rag/README|rag]] — RAG section landing.
- [[../agents/README|agents]] — agentic-AI section landing.

## Keywords

`#episodic-memory` `#em-llm` `#memorag` `#mamba` `#arigraph` `#ai-native-memory` `#agents`

## References / raw notes

### EM-LLM — Human-Like Episodic Memory for Infinite Contexts

The EM-LLM framework introduces a memory system inspired by human episodic memory, enabling LLMs to process effectively unlimited context lengths without retraining. It segments information into coherent events using **Bayesian surprise** and graph-theoretic methods, then retrieves through a two-stage process combining similarity-based and temporal searches. EM-LLM demonstrates retrieval over 10M tokens.

- Project page: <https://em-llm.github.io/>
- Paper: [arXiv 2407.09450](https://arxiv.org/abs/2407.09450)

### MemoRAG — Memory-Inspired Retrieval-Augmented Generation

MemoRAG enhances traditional RAG with a dual-system architecture: a lightweight long-range LLM forms a **global memory** to generate draft answers; those drafts guide retrieval to relevant information; a more expressive LLM produces the final response. Improves performance on tasks where conventional RAG falls short (ambiguous queries, multi-hop reasoning, very long documents).

- Paper: [arXiv 2409.05591](https://arxiv.org/abs/2409.05591)

### Mamba — Efficient Working-Memory Compression

Albert Gu's **Mamba** introduces a selective state-space mechanism that compresses prior tokens into a fixed-size hidden state, enabling linear-time sequence processing as an alternative to quadratic attention. Adaptive across modalities (language, audio, genetics). The Falcon-Mamba 7B model from TII is among the first production-grade open implementations.

- Time profile of Albert Gu: <https://time.com/7012853/albert-gu/>
- xLSTM (Beck et al. 2024) is the closest sibling research direction.

### AriGraph — Knowledge-Graph World Model + Episodic Memory

AriGraph proposes that agents construct and update a **memory graph** that integrates semantic facts and episodic events. The graph supports planning and reasoning in complex environments where decisions span extended interactions.

- Paper: [arXiv 2407.04363](https://arxiv.org/abs/2407.04363)

### AI-Native Memory — Towards Personalised AGI

The "AI-native memory" thesis (and its follow-up *Second Me / AI-native Memory 2.0*) envisions LLMs as core processors **supplemented by a small dedicated memory model** that stores conclusions, reasoning outputs, and semantically connected knowledge. The intermediate representation can be natural language ("Memory Palace") or compressed neural ("Lifelong Personal Model").

- AI-Native Memory: [arXiv 2406.18312](https://arxiv.org/abs/2406.18312)
- AI-Native Memory 2.0 / Second Me: arXiv 2503.08102

### Position paper — the open challenge

- Pink et al. 2025 — *Position: Episodic Memory is the Missing Piece for Long-Term LLM Agents.* arXiv 2025. Frames episodic memory as the under-served axis in current agentic-LLM research.
- DeChant 2025 — *Episodic memory in AI agents poses risks that should be studied and mitigated.* The other side of the coin: privacy, accountability, deletion.
