---
title: Building effective agents (Anthropic, Dec 2024)
main_link: https://www.anthropic.com/engineering/building-effective-agents
keywords: [anthropic, agents, workflows, augmented-llm, orchestrator-workers, evaluator-optimizer]
status: reviewed
---

# Building effective agents (Anthropic, Dec 2024)

**Main link:** <https://www.anthropic.com/engineering/building-effective-agents>

## Summary

Anthropic's December 2024 engineering post is the **single most-cited reference for agent design** in the post-ChatGPT era. It draws a sharp line between *workflows* (LLM calls wired together by humans, with predictable control flow) and *agents* (LLMs that decide their own steps and tool calls in a loop), then catalogues five workflow patterns and one agent pattern that cover the vast majority of useful systems. The piece is opinionated: start with the simplest thing that works, only escalate to autonomous agents when the task variability genuinely demands it.

## Insight

The article's most durable contribution is a **vocabulary** that the rest of the industry has adopted:

- **Augmented LLM** — the atomic unit: an LLM with retrieval, tools, and memory bolted on. Most "agent" systems are actually augmented LLMs in a loop.
- **Workflows** — predictable control flow:
  - **Prompt chaining** — sequential LLM calls, each consuming the previous output.
  - **Routing** — classify input, dispatch to a specialised handler.
  - **Parallelisation** — sectioning (split task) or voting (same task multiple times for confidence).
  - **Orchestrator-workers** — central LLM dynamically decomposes and delegates to worker LLMs.
  - **Evaluator-optimizer** — generator + critic in a loop until the critic is satisfied.
- **Agents** — open-ended, the LLM decides when to stop. Use only when the task space is too varied to chart out as a workflow.

**When to read it:** before you build *anything* agentic. The piece will save you from over-engineering: most production "agent" systems are honestly orchestrator-worker workflows that don't need to advertise themselves as agents. Anthropic's headline guidance — *"find the simplest solution and only increase complexity when needed"* — is the durable lesson.

**Vs the alternatives:** the older *ReAct* paper (Yao et al. 2022) is more popular but covers only the reason-act-observe loop in a single agent; *Building effective agents* covers the full design space and labels the patterns. Lilian Weng's *LLM Powered Autonomous Agents* (Jun 2023) is the academic counterpart; Anthropic's piece is the engineering counterpart.

## Similar / related topics

- **ReAct (Yao et al. 2022)** — the older reason-act loop paper; narrower scope.
- **Lilian Weng — *LLM Powered Autonomous Agents*** — academic-leaning survey from OpenAI.
- **OpenAI Agents SDK** — productised version of the orchestrator-workers pattern (see [[openai]]).
- **LangGraph** — graph-based orchestration that maps cleanly onto these workflow patterns.
- **DSPy** — pushes the abstraction up: compile prompts and weights for these patterns.

## Internal links

<!-- reviewed -->
- [[README]] — agents section landing.
- [[agent_instructs]] — per-repo instruction conventions (`.cursorrules` / `AGENTS.md` / `CLAUDE.md`).
- [[orchestration]] — the workflow patterns from this article, in framework form.
- [[openai]] — OpenAI's productisation of the orchestrator-workers pattern.
- [[smolagents]] — minimal library implementing the agent loop.
- [[../frameworks/langgraph|langgraph]] — graph-based orchestration of these patterns.
- [[../frameworks/dspy|dspy]] — prompt-and-weight compiler.
- [[../mcp/README|mcp]] — the tool-call ABI agents use.

## Keywords

`#anthropic` `#agents` `#workflows` `#augmented-llm` `#orchestrator-workers` `#evaluator-optimizer`

## References / raw notes

- Anthropic engineering blog: <https://www.anthropic.com/engineering/building-effective-agents>
- Companion repo with reference implementations: <https://github.com/anthropics/anthropic-cookbook/tree/main/patterns/agents>
- ReAct (Yao et al. 2022): <https://arxiv.org/abs/2210.03629>
- Lilian Weng, *LLM Powered Autonomous Agents* (Jun 2023): <https://lilianweng.github.io/posts/2023-06-23-agent/>
- Anthropic's later (Apr 2025) follow-up *How we built our multi-agent research system*: <https://www.anthropic.com/engineering/built-multi-agent-research-system> — the orchestrator-workers pattern applied at scale.
