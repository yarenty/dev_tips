---
title: Data Interpreter — LLM agent for end-to-end data science (paper summary)
main_link: https://arxiv.org/abs/2402.18679
keywords: [data-interpreter, agents, data-science, hierarchical-graph, metagpt, paper]
status: reviewed
review_date: 2026/05/03
---

# Data Interpreter — LLM agent for end-to-end data science (paper summary)

**Main link:** <https://arxiv.org/abs/2402.18679>

## Summary

Paper summary of *Data Interpreter: An LLM Agent For Data Science* (Hong et al., DeepWisdom / MetaGPT team, Feb 2024 — arXiv 2402.18679). Data Interpreter is an LLM-based agent that solves data science problems end-to-end (loading, cleaning, EDA, modelling, evaluation) by representing the workflow as a **Hierarchical Graph** of tasks and actions, with **Programmable Node Generation** that iteratively refines and verifies each step. Reported gains include +25% on InfiAgent-DABench, comprehensive score 0.95 on ML-Benchmark (vs AutoGen / OpenDevin), and 0.97 completion on open-ended tasks. The implementation lives inside the [MetaGPT](https://github.com/FoundationAgents/MetaGPT) framework.

## Insight

The interesting bit is the **task-graph + action-graph split**: a high-level DAG of *tasks* (load → clean → train → evaluate) sits on top of an action graph where each *action node* is an executable code snippet integrating tools. Iterative graph refinement adjusts the task DAG as the environment changes (failures, schema discoveries, user clarifications), and programmable node generation re-tries each action with verification. This is the architectural pattern that later showed up in many agentic-data-science products (Numbers Station, Julius, Hex Magic, Decoda, Vanna AI's DataAgent). The 2024 paper is now "the canonical reference for agentic-DS architecture" — even if the specific reported numbers are best treated as upper-bounds (Kaggle-grade benchmarks tend to be optimised against).

When to read it:
- **You're building an agentic data-science pipeline** — the hierarchical-graph framing maps cleanly onto LangGraph / Burr / CrewAI's planner+executor split.
- **You want a structured baseline** to compare your own agent design against.
- **You're studying the MetaGPT lineage** of papers (Data Interpreter → SELA → Co-scientist-style agent stacks).

When to skip:
- You just want a working data-science copilot — reach for [Cursor](../ml/tools/cursor.md) + Jupyter or one of the productised tools above; this paper is architectural background, not a tutorial.

## Similar / related topics

- **AutoGen** (Microsoft Research, 2023) — the conversational-multi-agent baseline Data Interpreter benchmarks against.
- **OpenDevin / OpenHands** — the open-source coding agent that Data Interpreter outperforms on ML-Benchmark; see [[../ml/agents/devin|devin]].
- **MetaGPT** — the parent framework Data Interpreter ships inside (Hong et al., 2023, ICLR 2024).
- **AIDE** (Schmidt et al., 2024) — Sakana AI's Kaggle agent with similar tree-search-over-code-edits framing; complementary architectural choice.
- **InfiAgent-DABench** (Hu et al., 2024) — the data-analysis benchmark used; useful sanity-check dataset if you're building your own agent.

## Internal links

- [[../ml/agents/co_scientist|co_scientist]] — Google's multi-agent scientific-reasoning system; same hierarchical-pipeline pattern in the science domain.
- [[../ml/agents/coding_agents|coding_agents]] — broader landscape of agentic coding/data tools.
- [[../ml/agents/orchestration|agents/orchestration]] — LangGraph and friends, the modern substrate for the kind of graph-based agent control flow this paper proposes.
- [[../ml/orchestration|ml/orchestration]] — ML pipeline / workflow orchestration (Airflow / Prefect / Dagster), the data-engineering counterpart to the agentic approach.
- [[../ml/frameworks/dspy|dspy]] — declarative programming of LLM pipelines; an alternative compile-time approach to the same problem.
- [[../ml/agents/futurehouse|futurehouse]] — open scientific-research agent stack (PaperQA / Crow), another instance of multi-agent scientific reasoning.

## Keywords

`#paper` `#agents` `#data-science` `#hierarchical-graph` `#metagpt` `#deepwisdom` `#llm-agents`

## References / raw notes

- **Paper:** Hong, S. et al. *Data Interpreter: An LLM Agent For Data Science.* arXiv:2402.18679 (Feb 2024). <https://arxiv.org/abs/2402.18679>
- **Code:** part of MetaGPT — <https://github.com/FoundationAgents/MetaGPT>
- **Affiliated team:** DeepWisdom (the MetaGPT authors).

### Distilled paper notes

**Abstract / contributions**
- LLM agent for end-to-end data science (loading → cleaning → EDA → modelling → evaluation).
- Hierarchical Graph Modeling for complex-task decomposition.
- Programmable Node Generation for iterative refinement and verification of each code step.
- +25% on InfiAgent-DABench; 0.95 comprehensive score on ML-Benchmark vs AutoGen / OpenDevin; 0.97 completion on open-ended tasks; +26.5% relative on MATH vs AutoGen.

**Methodology**
- Hierarchical Graph Modeling: data-science problem decomposed into manageable tasks (high-level nodes) and actions (executable code-snippet nodes); edges represent dependencies.
- Task Graph: dynamically refined as the agent observes failures or schema surprises; managed by a task-graph generator that adds / removes / modifies tasks.
- Action Graph (Programmable Node Generation): each action node is a code snippet that integrates the right tools for the task; verification step runs the snippet and decides whether to keep or retry.

**Benchmarks used**
- **InfiAgent-DABench** — data-analysis tasks.
- **ML-Benchmark** — real-world ML challenges.
- **Open-ended task benchmark** — dynamic data-handling.
- **MATH** — mathematical problem-solving sanity check.

**Ablation**
- Iterative graph refinement: +0.48 on the comprehensive score.
- Programmable node generation: further +10.6%, reaching 0.94.
- Improves further with GPT-4o / GPT-4o-mini.

**Limitations the paper acknowledges**
- Datasets used are entry-level Kaggle; diversity is limited.
- No precise self-improvement loop using numerical feedback across runs.
- Mathematical evaluation truncated by API budget.

**Broader-impact note**
- Reduces cost of customised data-science work; flexibility comes with security risk if users feed in malicious code; mitigation = pre-generation code-checking + safety-policy-aligned LLMs.
