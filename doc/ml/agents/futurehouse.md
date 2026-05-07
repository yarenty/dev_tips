---
title: FutureHouse — open scientific-research agents (PaperQA, Crow, Falcon, Owl, Phoenix)
main_link: https://www.futurehouse.org/
keywords: [futurehouse, paperqa, crow, falcon, owl, phoenix, scientific-agents]
status: reviewed
---

# FutureHouse — open scientific-research agents

**Main link:** <https://www.futurehouse.org/>

## Summary

**FutureHouse** is a non-profit research organisation (founded by Sam Rodriques, ex-MIT, with Eric Schmidt's backing) building **open AI agents for scientific research**. Their flagship platform (May 2025 launch) bundles a portfolio of specialised agents: **Crow** (general literature Q&A), **Falcon** (deep literature reviews with access to specialised scientific databases like OpenTargets), **Owl** (the *"has anyone done X before?"* novelty checker), and **Phoenix** (chemistry experiment planning, formerly ChemCrow). The substrate is the open-source **PaperQA** library (their original release), which does retrieval-augmented Q&A grounded in scientific PDFs with proper citation tracking. FutureHouse is the **open counterpart to Google's [[co_scientist]]**.

## Insight

What makes FutureHouse interesting is the combination of **non-profit funding model** (so they can publish openly), **specialised per-task agents** (rather than one omni-agent), and **honest grounding** (every claim is cited back to a paper, with quote-level provenance):

- **PaperQA** is the durable contribution. It's a small Python library that does RAG-over-papers properly: it indexes PDFs, retrieves relevant passages, and synthesises an answer with inline citations the user can verify. Independently it's *the* reference implementation for citation-grounded scientific RAG, used well outside FutureHouse.
- **The agent portfolio reflects how scientific research actually decomposes.** Different sub-tasks (broad literature search vs deep review vs novelty check vs experimental planning) want different agents with different tools, prompts, and quality bars. This is a more honest decomposition than "the AI scientist" omni-agent fantasy.
- **Open-source posture.** Most code is on GitHub (<https://github.com/Future-House>); papers are on arXiv; the platform has free tier API access. Compare to Google's co-scientist (closed, Trusted Tester only).
- **Wikicrow** (Sept 2024) automatically generates Wikipedia-quality articles for human protein-coding genes — a concrete use-case demonstrating PaperQA's range.

**When to reach for FutureHouse:** if you're doing computational scientific Q&A or literature review and need **citation provenance**, this is the substrate. The platform itself is useful for biomedical / chemistry researchers; the libraries (PaperQA, ldp, paper-search) are useful well beyond that.

**Honest caveats.** Like all current agentic-research tools, accuracy on cutting-edge or contested claims is mixed; treat outputs as a research aid, not a replacement for a domain expert. Coverage is biased toward biomedical / chemistry. The platform layer is more polished than the libraries (which are research-grade Python).

## Similar / related topics

- **Google AI co-scientist** — closed, multi-agent scientific reasoning; see [[co_scientist]].
- **PaperQA / PaperQA2** — FutureHouse's own RAG-over-papers library.
- **Sakana AI Scientist** — autonomous end-to-end paper generation (Aug 2024).
- **Stanford CRFM Virtual Lab** — multi-agent biomedical hypothesis exploration.
- **ChemCrow** — earlier (2023) chemistry agent that became FutureHouse's Phoenix.
- **Elicit / Consensus / Scite** — commercial scientific Q&A tools (less agentic, more search-with-citations).

## Internal links

<!-- reviewed -->
- [[README]] — agents section landing.
- [[co_scientist]] — Google AI co-scientist (closed counterpart).
- [[claude]] — Anthropic's *Building effective agents* (the orchestrator-workers pattern this uses).
- [[orchestration]] — multi-agent orchestration in framework form.
- [[../rag/agentic_rag|agentic_rag]] (P5.AE) — agentic-RAG patterns (PaperQA is a clean instance).
- [[../rag/summary_of_best_practices|rag/best practices]] (P5.AE) — citation-grounded RAG sits in this tradition.
- [[../knowledge_graph/papers|kg/papers]] (P5.AC) — KG-side reading list.

## Keywords

`#futurehouse` `#paperqa` `#scientific-agents` `#open-source` `#non-profit-research`

## References / raw notes

- Org: <https://www.futurehouse.org/>
- Platform launch announcement (May 2025): <https://www.futurehouse.org/research-announcements/launching-futurehouse-platform-ai-agents>
- GitHub: <https://github.com/Future-House>
- PaperQA (the headline library): <https://github.com/Future-House/paper-qa>
- PaperQA2 paper (2024): <https://arxiv.org/abs/2409.13740> *Aviary: Training Language Agents on Challenging Scientific Tasks.*
- Wikicrow (Sept 2024): <https://www.futurehouse.org/research-announcements/wikicrow>

### Agent portfolio (per the FutureHouse Platform announcement)

- **Crow** — general-purpose agent that searches the literature and provides concise scholarly answers; suited for API use.
- **Falcon** — specialised for deep literature reviews; can search and synthesise more scientific literature than any other agent the team is aware of, with access to specialised scientific databases like OpenTargets.
- **Owl** (formerly **HasAnyone**) — answers *"Has anyone done X before?"* — the explicit novelty / prior-art question.
- **Phoenix** (experimental) — deployment of **ChemCrow**; an agent with access to specialised tools that helps plan chemistry experiments.
- **Wikicrow** — auto-generates Wikipedia-quality articles for human protein-coding genes.

### Adjacent open work

- ChemCrow (Bran et al. 2023): <https://arxiv.org/abs/2304.05376>
- Sakana AI Scientist (Aug 2024): <https://sakana.ai/ai-scientist/>
- Stanford CRFM *Virtual Lab* (Nov 2024): <https://crfm.stanford.edu/2024/11/22/virtual-lab.html>
