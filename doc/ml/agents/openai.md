---
title: OpenAI Agents SDK & Responses API (formerly Swarm)
main_link: https://openai.github.io/openai-agents-python/
keywords: [openai, agents-sdk, responses-api, swarm, operator, computer-use]
status: reviewed
---

# OpenAI Agents SDK & Responses API

**Main link:** <https://openai.github.io/openai-agents-python/>

## Summary

OpenAI's official agent stack — released in March 2025 — is an evolution of the experimental **Swarm** library (Oct 2024) into a production-named **Agents SDK** plus the new **Responses API** (a successor to Chat Completions designed around stateful tool use). Together with **Computer Use** (browser/desktop control), **Operator** (OpenAI's hosted browser agent), and the older **Function Calling** primitive, this is OpenAI's coherent answer to "how should developers build agents on our models". The SDK targets the *orchestrator-workers* pattern from [[claude]]: a primary agent with hand-offs and tool calls to specialised sub-agents.

## Insight

The pieces and how they fit:

- **Function calling** (Jun 2023) — the original primitive: model emits a structured tool-call JSON. Still the default for simple tool use.
- **Assistants API** (Nov 2023, *deprecated 2026*) — first attempt at managed agent state with threads + tools. Being phased out in favour of the Responses API.
- **Swarm** (Oct 2024, archived) — an *experimental* multi-agent library with the *handoffs* + *routines* abstractions. Not for production but established the design language.
- **Responses API** (Mar 2025) — the new core API: stateful conversations, built-in tool support (file search, computer use, web search), structured outputs first-class. Designed for agent loops.
- **Agents SDK** (Mar 2025, productionised Swarm) — Python SDK on top of Responses API providing `Agent`, `Runner`, handoffs, guardrails, tracing. Released with a TypeScript port shortly after.
- **Computer Use** (Oct 2024 research preview, GA via Responses API) — browser/desktop control via screenshots-and-clicks; the substrate for Operator.
- **Operator** (Jan 2025, USA only initially) — OpenAI's hosted browser agent product running on Computer Use.

**When to reach for it:** if you're already on OpenAI for the model, the Agents SDK is the path-of-least-resistance, with first-class tracing in the OpenAI dashboard and tight Responses-API integration. The handoffs pattern is genuinely well-designed and maps cleanly onto Anthropic's orchestrator-workers ([[claude]]).

**Vs the alternatives.** [LangGraph](../frameworks/langgraph.md) is more general (any model, any provider, more control) but more code. CrewAI is higher-level (role-based, fewer knobs). Anthropic's recommended pattern (Claude + Tool Use API) is more minimalist but you write the agent loop yourself. [[smolagents|HF smolagents]] and [[../llm/runtimes/goose|Goose]] are the open-substrate alternatives.

**Honest caveats.**

- **Lock-in.** Agents SDK + Responses API ties you tightly to OpenAI's API surface; switching providers means a rewrite. Some users mitigate by using the Agents SDK as an architectural pattern but routing through LiteLLM.
- **Computer Use is impressive but slow and expensive.** Per-step latency is in seconds; cost per task is dollars. Use only where the alternative (your own scraper / API integration) is genuinely impossible.
- **Operator is gated** — a consumer subscription, not a developer API; the developer-side equivalent is Computer Use through the Responses API.
- **The Assistants API deprecation timeline matters** — if you built on Assistants API in 2024, you have a migration in your future.

## Similar / related topics

- **Anthropic Tool Use + Claude** — the substrate-level peer; you build the loop, Anthropic's docs (see [[claude]]) tell you how.
- **Anthropic Computer Use** — Anthropic's equivalent of Computer Use, released Oct 2024.
- **Google Gemini ADK / Vertex AI Agent Builder** — Google's productised agent stack.
- **LangGraph** — provider-neutral graph-based orchestration; see `[[../frameworks/langgraph|langgraph]]`.
- **smolagents** — open Python library with the code-agents pattern; see [[smolagents]].
- **CrewAI / AutoGen** — higher-level multi-agent frameworks.

## Internal links

<!-- reviewed -->
- [[README]] — agents section landing.
- [[claude]] — Anthropic's *Building effective agents* (the design framing).
- [[orchestration]] — orchestration patterns and frameworks.
- [[smolagents]] — open Python alternative.
- [[agent_instructs]] — `AGENTS.md` convention (introduced by OpenAI Codex CLI).
- [[../mcp/README|mcp]] — Model Context Protocol (Anthropic-led standard, now also in Agents SDK).
- [[../frameworks/langgraph|langgraph]] (P5.AB) — provider-neutral graph orchestration.
- [[../frameworks/dspy|dspy]] (P5.AB) — prompt-and-weight compiler.
- [[../llm/runtimes/providers|providers]] (P5.AD) — wider LLM provider landscape.

## Keywords

`#openai` `#agents-sdk` `#responses-api` `#swarm` `#operator` `#computer-use`

## References / raw notes

- Agents SDK Python docs: <https://openai.github.io/openai-agents-python/>
- Agents SDK announcement (Mar 2025): <https://openai.com/index/new-tools-for-building-agents/>
- Responses API docs: <https://platform.openai.com/docs/api-reference/responses>
- Responses-API function-calling guide: <https://platform.openai.com/docs/guides/function-calling?api-mode=responses>
- Swarm (archived but historically informative): <https://github.com/openai/swarm>
- Operator: <https://openai.com/index/introducing-operator/>
- Computer Use system card: <https://openai.com/index/computer-using-agent/>
- Codex CLI (the agent CLI from the same team): <https://github.com/openai/codex>

### Architectural pattern (Agents SDK)

```python
from agents import Agent, Runner

assistant = Agent(
    name="Assistant",
    instructions="You are a helpful assistant.",
    tools=[search_tool, calculator_tool],
)

specialist = Agent(
    name="MathSpecialist",
    instructions="You only do hard maths.",
)

assistant.handoffs = [specialist]

result = Runner.run_sync(assistant, "What is 1234567 * 7654321?")
print(result.final_output)
```

The two key abstractions:

- **Handoffs** — one agent can hand the conversation to another, like a workflow router.
- **Tools** — Python callables decorated as tools; Pydantic types give structured I/O for free.
