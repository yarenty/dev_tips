---
title: Agents — autonomous LLM-driven systems
keywords: [agents, llm, orchestration, coding-agents, autonomous]
status: reviewed
---

# Agents — autonomous LLM-driven systems

This section catalogues the **agentic-AI** landscape: what an "agent" is in 2024-2025 (an LLM in a tool-use loop, optionally with memory and planning), the headline products and frameworks, and the per-repo conventions that have grown up around them. The space moves fast — every article tries to give a *durable* framing alongside the current state.

The companion sections are deliberately split out for graph-friendliness:

- `[[../mcp/README|mcp]]` — the **tool-side ABI** (Model Context Protocol) that agents talk to.
- `[[../llm/README|llm]]` — the **models and runtimes** the agent sits on top of.
- `[[../rag/README|rag]]` — the **retrieval** sub-discipline that's increasingly fused with agents.
- `[[../memory/README|memory]]` — the **memory architectures** complementary to `agents/memory`.
- `[[../frameworks/README|frameworks]]` — DSPy / LangGraph / phidata / etc. (the framework-side, not the agent-side).
- `[[../finetuning/README|finetuning]]` — when prompting + tools isn't enough.

## Foundational reading

- [[claude]] — Anthropic's *Building effective agents* (Dec 2024). **Read this first.** Workflow vs agent, the augmented LLM, prompt-chaining / routing / parallelisation / orchestrator-workers / evaluator-optimiser.
- [[agent_instructs]] — the `.cursorrules` / `AGENTS.md` / `CLAUDE.md` / `.goosehints` per-repo override convention. The system-prompt-as-source-controlled-file pattern.

## Closed / commercial agents

- [[openai]] — OpenAI Agents SDK (Mar 2025, formerly Swarm), Responses API, Operator, Computer Use.
- [[devin]] — Devin (Cognition AI, closed) **and** OpenDevin / OpenHands (the open-source clone).
- [[uipath]] — UiPath Agent Builder + Maestro (RPA-vendor pivot to AI agents).

## Coding-agent subfamily

Coding agents deserve their own subsection because the loop (`read code → plan → edit → run tests → iterate`) is so well-defined.

- [[coding_agents]] — landscape (Cursor / Cline / Aider / Windsurf / Copilot Workspace / Codex CLI / Claude Code / Continue.dev / Amp / OpenCode / DeepCode).
- [[devin]] — Devin + OpenHands.
- [[devon]] — entropy-research/Devon (Python-only, separate project from Devin).
- [[emdash]] — cross-platform UI for **running multiple coding-agent CLIs in parallel** (each in its own git worktree).
- See also: `[[../llm/runtimes/goose|goose]]` (open agent CLI), `[[../llm/runtimes/extensions|goose extensions]]` (`.goosehints`).

## Open-source agent libraries / frameworks

- [[smolagents]] — HuggingFace's lightweight library, **code-agents pattern** (LLM emits Python, not JSON tool-calls). ~1k LOC of agent logic.
- [[hf]] — older HuggingFace agent tooling + tutorials index.
- [[../frameworks/langgraph|langgraph]] (P5.AB) — stateful, cyclic agent graphs (LangChain spinout).
- [[../frameworks/dspy|dspy]] (P5.AB) — prompt-and-weight compiler (Stanford NLP).
- [[../frameworks/phidata|phidata]] (P5.AB, **agno rebrand**) — agent + RAG framework with playground/observability.

## Domain-specific (scientific) agents

- [[co_scientist]] — Google's AI co-scientist (Feb 2025): Generation / Reflection / Ranking / Evolution / Proximity / Meta-review multi-agent pipeline for hypothesis generation.
- [[futurehouse]] — FutureHouse's PaperQA / Crow / Falcon / Owl / Phoenix / Wikicrow (open scientific-research agents).

## Agent internals

- [[memory]] — agent-memory **patterns** (scratchpad / long-term store / episodic / procedural). Cross-link `[[../memory/README|ml/memory]]` (P5.AC) for the **architecture** side.
- [[orchestration]] — workflow orchestration (LangGraph, CrewAI, AutoGen, Burr, Parlant). Note: `ml/orchestration/` (P5.AG upcoming) covers a different angle.
- [[task]] — agent-task framing notes.

## Misfiled but kept here for hyperlink stability

- [[anemll]] — Apple Neural Engine LLM library. Really an **on-device LLM runtime** (closer in spirit to `[[../llm/runtimes/README|llm/runtimes]]` from P5.AD), not an agent. The article points readers there explicitly.

## Decision shortcut

| If you want… | Reach for |
|---|---|
| The canonical "what is an agent" reference | [[claude]] |
| A per-repo override file the agent will read | [[agent_instructs]] |
| OpenAI's official agent stack | [[openai]] |
| The closed autonomous coding agent | [[devin]] (Cognition Devin) |
| The open clone of the above | [[devin]] (OpenHands section) |
| A UI to run several coding-agent CLIs side-by-side | [[emdash]] |
| Lightweight Python agent library, code-agents pattern | [[smolagents]] |
| Stateful graph orchestration with HITL & checkpoints | [[../frameworks/langgraph\|langgraph]] |
| Multi-agent scientific reasoning (Google flavour) | [[co_scientist]] |
| Multi-agent scientific reasoning (open / non-profit) | [[futurehouse]] |
| RPA-shaped enterprise agent platform | [[uipath]] |
| Apple-Silicon on-device LLM (not really an agent) | [[anemll]] |
| Per-repo agent instruction conventions | [[agent_instructs]] |
| Agent **memory** patterns | [[memory]] + [[../memory/README\|ml/memory]] |
| Agent **orchestration** frameworks | [[orchestration]] + [[../frameworks/langgraph\|langgraph]] |

## See also

- [[../README|ml]] — top of the ML hub.
- [[../llm/README|llm]] — models and runtimes.
- [[../mcp/README|mcp]] — Model Context Protocol (the tool-call ABI).
- [[../rag/README|rag]] — retrieval-augmented generation.
- [[../memory/README|memory]] — memory architectures.
- [[../frameworks/README|frameworks]] — orchestration / autoML / SQL-ML.
- [[../finetuning/README|finetuning]] — when prompting and tools aren't enough.
- [[../llm/system_prompts/README|system_prompts]] (P5.AD) — system-prompt patterns and leaks.
- [[../llm/runtimes/goose|goose]] (P5.AD) — Block's open agent CLI.

## Keywords

`#agents` `#llm` `#orchestration` `#coding-agents` `#autonomous`
