---
title: Agent task framing — how to specify what an agent should do
main_link: https://www.anthropic.com/engineering/building-effective-agents
keywords: [agent-task, task-framing, prompt-engineering, scope, agent-design]
status: reviewed
---

# Agent task framing

**Main link:** <https://www.anthropic.com/engineering/building-effective-agents>

## Summary

This is a small companion article to [[memory]] and [[../memory/response|ml/memory/response]] — the original `agents/task.md` was the *prompt* that produced the eidetic-memory survey, and is preserved here as a worked example of **agent task framing**: how the way you specify the question to an agent shapes the answer, the search budget, and the citation discipline. The broader insight — task framing is the cheapest agent-design lever — generalises far beyond memory surveys.

## Insight

Two practical levers, both underused:

1. **Be explicit about scope, freshness, and source-quality bar.** The original survey question succeeded because it specified the topic (*Agentic AI / RAG / memory / eidetic*), the source archives (*arXiv, PubMed*), and the time window (*2024-2025*). Agents that get vague questions go in vague directions; small additions ("with citations", "from peer-reviewed sources only", "no marketing pages") meaningfully steer outcomes.
2. **Decompose before you delegate.** The Anthropic *orchestrator-workers* pattern ([[claude]]) shines when the orchestrator does explicit task decomposition before assigning to workers. *Generation → Reflection → Ranking → Evolution → Proximity → Meta-review* (Google's [[co_scientist]] pipeline) is a worked example of this; *retrieve → rerank → generate → critique* is the RAG variant.

**Common task-framing mistakes:**

- Asking an open-ended question ("research X") rather than a closed one ("identify all 2024-2025 papers proposing memory architectures for agents that explicitly target eidetic recall").
- Leaving the *output format* unspecified — agents that don't know what shape the deliverable should take produce mush.
- Leaving the *budget* unspecified — agents that don't know if they have 10 or 1000 tool calls behave wildly differently.
- Conflating "task" with "goal" — the goal is what the user wants; the task is what the agent does. Decompose.

The DSPy ([[../frameworks/dspy|dspy]]) abstraction of **Signatures** (`question, context -> answer`) is a clean way to enforce explicit task framing in code; the LangGraph state-graph approach is a more flexible cousin.

## Similar / related topics

- **[[memory]]** — the agent-memory companion that preserves the original raw question.
- **[[claude]]** — Anthropic's task-decomposition patterns.
- **[[co_scientist]]** — multi-agent task decomposition done well.
- **DSPy Signatures** — formalised task framing as code.
- **LangChain output parsers** — the *output format* lever in code.

## Internal links

- [[README]] — agents section landing.
- [[memory]] — sibling article preserving the original raw question.
- [[claude]] — orchestrator-workers and evaluator-optimizer patterns.
- [[orchestration]] — orchestration frameworks that own task lifecycle.
- [[co_scientist]] — multi-agent scientific-task decomposition.
- [[../memory/response|ml/memory/response]] (P5.AC) — the substantive answer this question produced (the eidetic-memory survey).
- [[../frameworks/dspy|dspy]] (P5.AB) — formalises task framing via Signatures.

## Keywords

`#agent-task` `#task-framing` `#prompt-engineering` `#scope` `#agent-design`

## References / raw notes

### The original raw question (preserved as a worked task-framing example)

> Has anyone looked into Agentic AI / RAG and memory solutions — could eidetic memory be a great fit for Agents/RAG/LLM? Try to find examples on arXiv or PubMed; focus on memory issues and solutions, especially within the latest years (2024/2025).

What this question got *right* as a piece of agent task framing:

- **Topic** specified (Agentic AI / RAG / memory / eidetic).
- **Sources** named (arXiv, PubMed) — the agent knows where to look.
- **Time window** named (2024-2025) — the agent knows what to ignore.
- **Output focus** implied (issues + solutions, not just papers).

What it could have specified more sharply:

- **Output format** (table? annotated bibliography? prose review?).
- **Budget** (how many papers? how many tool calls?).
- **Quality bar** (peer-reviewed only? include preprints?).
- **Citation requirement** (verbatim quotes? page numbers?).

The substantive answer the question produced is preserved as **[[../memory/response|ml/memory/response]]** (P5.AC) — the eidetic-memory 2024-2025 survey.

### See also

- Anthropic *Building effective agents*: <https://www.anthropic.com/engineering/building-effective-agents>
- The FoundationAgents memory section that contextualised the original question: <https://github.com/FoundationAgents/awesome-foundation-agents#memory>
