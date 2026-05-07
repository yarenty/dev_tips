---
title: WebThinker — open-source LRM-powered deep-research agent
main_link: https://github.com/RUC-NLPIR/WebThinker
keywords: [webthinker, ruc, nlpir, deep-research, lrm, open-source, agent, search, dpo]
status: reviewed
---

# WebThinker — open-source LRM-powered deep-research agent

**Main link:** <https://github.com/RUC-NLPIR/WebThinker>

## Summary
WebThinker (RUC NLPIR) is an open-source **deep-research framework powered by large reasoning models** (LRMs). The reasoning model itself drives end-to-end research within a single generation — searching, navigating web pages by clicking interactive elements, extracting information, and drafting/checking/editing a structured report — instead of being orchestrated by a fixed RAG-style workflow. The team is also experimenting with online-DPO training on reasoning trajectories to improve task performance over time.

## Insight
Reach for WebThinker (and the surrounding "deep-research" ecosystem) when the brief is **"have an LLM go off and produce a multi-page, sourced report on X" rather than "answer one question from one document"**. The architectural bet is what makes it interesting: instead of the standard agentic-RAG loop (LLM → retrieval → LLM → retrieval → …), the LRM keeps the search and synthesis *inside* its own thinking process. The "Deep Web Explorer" (search + click links/buttons + extract) and the "Autonomous Think-Search-and-Draft" loop (draft a chapter → check the report → edit) are the two distinctive sub-systems.

Frame it as the **open alternative to closed deep-research products**:
- **vs OpenAI Deep Research** (in ChatGPT Plus / Pro) — closed, polished, generally produces better-structured reports with stronger citation habits.
- **vs Anthropic Claude Research** — closed; similar polish gap.
- **vs Perplexity Pro Research** / Perplexity Spaces — closed, more search-engine-shaped.
- **vs Google Gemini Deep Research** — closed; tightly integrated with Google search.
- **vs [`gpt-researcher`](https://github.com/assafelovic/gpt-researcher)** — earlier open-source deep-research project; more workflow-driven (planner → researcher → publisher), less reasoning-model-centric.
- **vs [MindSearch](https://github.com/InternLM/MindSearch)** (InternLM) — multi-agent web search; similar lineage.
- **vs [OpenManus](https://github.com/mannaandpoem/OpenManus)** — open clone of the closed Manus agent product; broader scope than research alone.
- **vs [Stanford STORM](https://github.com/stanford-oval/storm)** — Wikipedia-style article generation from web sources; the closest academic precursor.

Honest "early, results variable" framing applies. Open deep-research agents are improving fast (especially when paired with strong reasoning models like DeepSeek-R1, Qwen-QwQ, or Kimi-K1.5), but the gap to closed frontier products on long-horizon coherence and citation discipline is still real. WebThinker's online-DPO-on-trajectories direction is genuinely interesting research; whether the result reproduces well outside the authors' setup is the usual open-research question.

Cross-link to the agentic-research neighbours covered in this vault: [[../agents/co_scientist|co_scientist]] (Google) and [[../agents/futurehouse|futurehouse]] (open scientific agents — PaperQA, Crow, Falcon, Owl, Phoenix). Those approach the same "agent that does research" problem from a science-of-research angle, while WebThinker is web-research-shaped.

## Similar / related topics
- [[../agents/co_scientist|co_scientist]] — Google's AI co-scientist; multi-agent scientific hypothesis generation.
- [[../agents/futurehouse|futurehouse]] — open scientific agents (PaperQA, Crow, Falcon, Owl, Phoenix).
- [`gpt-researcher`](https://github.com/assafelovic/gpt-researcher) — earlier open-source deep-research project.
- [MindSearch](https://github.com/InternLM/MindSearch) — InternLM's multi-agent web search.
- [Stanford STORM](https://github.com/stanford-oval/storm) — academic precursor for Wikipedia-style article generation.

## Internal links
- [[../agents/co_scientist|co_scientist]]
- [[../agents/futurehouse|futurehouse]]
- [[../agents/README|agents]]
- [[../rag/agentic_rag|agentic_rag]]
- [[../llm/models/deepseek|deepseek]]

## Keywords
`#webthinker` `#deep-research` `#lrm` `#open-source` `#agent` `#search` `#dpo`

## References / raw notes

WebThinker is a deep-research framework fully powered by large reasoning models (LRMs). It enables LRMs to autonomously search, deeply explore web pages, and draft research reports — all within their thinking process.

Unlike existing open-source deep-search agents that typically employ RAG with predefined workflows, WebThinker allows the reasoning model itself to perform actions during thinking, achieving end-to-end task execution in a single generation.

### Key features

- **Deep Web Explorer** — empowers LRMs to search, navigate pages by clicking interactive elements (links, buttons), and extract relevant information. Based on initial search results, the LRM can initiate follow-up searches and traverse deeper links until it has gathered all relevant information.
- **Autonomous Think-Search-and-Draft strategy** — for scientific reporting, integrates real-time knowledge seeking with report creation. Three specialised tools: (1) draft content for specific chapters; (2) check the current report; (3) edit the report — keeping it comprehensive, coherent, and adaptive to new insights.
- **RL-based training** (in progress) — optimise end-to-end task performance using reasoning trajectories from complex tasks. Accuracy of reasoning, tool usage, and final outputs are used to construct preference pairs for **online DPO training**, enabling progressive self-improvement.
