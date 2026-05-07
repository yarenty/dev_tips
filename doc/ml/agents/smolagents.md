---
title: smolagents — HuggingFace's lightweight code-agents library
main_link: https://huggingface.co/docs/smolagents/
keywords: [smolagents, huggingface, code-agents, sandboxed, e2b, lightweight]
status: reviewed
review_date: 2026/05/03
---

# smolagents — HuggingFace's lightweight code-agents library

**Main link:** <https://huggingface.co/docs/smolagents/>

## Summary

**smolagents** (HuggingFace, December 2024) is a deliberately tiny Python library for building agents — the core agent logic fits in roughly 1,000 lines of code. Its distinctive design choice is the **code-agents pattern**: instead of asking the LLM to emit a JSON tool-call, smolagents asks it to emit Python code, which is then executed in a sandbox (E2B, Docker, or local). The argument: code is what LLMs are most fluent in, naturally expresses control flow (loops, conditionals, composition), and avoids the JSON-parsing brittleness of structured-tool-calling. Model-agnostic (transformers, ollama, OpenAI, Anthropic, anything via LiteLLM); modality-agnostic (text/vision/video/audio); tool-agnostic (LangChain tools, MCP servers, HF Spaces).

## Insight

The headline insight is **tool-call as code, not as JSON**:

| Approach | What the LLM emits | Strengths | Weaknesses |
|---|---|---|---|
| **JSON tool calls** (OpenAI/Anthropic default) | `{"name": "search", "args": {"q": "x"}}` | Structured, validated, easy to log | Brittle on complex args; one tool per call; awkward composition |
| **Code agents** (smolagents, OpenInterpreter) | `result = search(q="x"); related = search(q=result[0].topic)` | Composable, naturally loops/branches, leverages LLM coding fluency | Needs sandboxing, larger security surface, harder to constrain |
| **Tool-as-DSL hybrids** (DSPy, Letta) | Per-system DSL | Fine-grained control | Each new DSL is a learning step |

Code agents win on **expressiveness** for tasks that involve multi-step composition (do X, look at the result, then do Y or Z). They lose on **enforceable constraints** — once the LLM is writing Python, it can do anything in the sandbox; you need real isolation.

**When to reach for smolagents.**

- You're already in the HuggingFace ecosystem and want minimal additional dependencies.
- Your tasks involve composing several tools where the composition logic varies (research, data analysis, multi-source RAG).
- You want to swap LLM providers easily (smolagents is provider-neutral via LiteLLM).
- You want to read the source — at ~1k LOC, smolagents is genuinely understandable end-to-end (compare LangGraph or AutoGen).

**When to look elsewhere.**

- You need stateful, cyclic graphs with checkpointing and HITL → **[[../frameworks/langgraph|LangGraph]]**.
- You need role-based multi-agent ensembles → **CrewAI** or **AutoGen**.
- You need a managed agent product on OpenAI → **OpenAI Agents SDK** ([[openai]]).
- You need a CLI rather than a library → **Goose** (`[[../llm/runtimes/goose|goose]]`), Aider, Claude Code.

**Sandboxing matters.** The default in-process Python sandbox is *not* secure against adversarial inputs; for any production use, configure E2B (their own [E2B](https://e2b.dev/) integration) or Docker. The HF docs are explicit about this; treat the warning as load-bearing.

**Model recommendations.** smolagents works with smaller open models (Qwen2.5-Coder-32B, DeepSeek-Coder-V2) — code-agents stress the *code-generation* skill, which is what coder-tuned models excel at. Anthropic Claude and OpenAI GPT-4o-class are the safe defaults.

## Similar / related topics

- **HuggingFace Agents Course** — the educational on-ramp; see [[hf]].
- **OpenInterpreter** — the older code-agents project; broader-scope (whole-OS automation), less HF-coupled.
- **LangGraph** — graph-state orchestration; provider-neutral; more code; see `[[../frameworks/langgraph|langgraph]]`.
- **CrewAI / AutoGen** — role-based multi-agent alternatives.
- **OpenAI Agents SDK** — productised agent stack on OpenAI.
- **DSPy** — different abstraction (compiler over prompts); see `[[../frameworks/dspy|dspy]]`.

## Internal links

- [[README]] — agents section landing.
- [[hf]] — HuggingFace agent docs / course (on-ramp).
- [[claude]] — Anthropic's *Building effective agents* (the workflow patterns smolagents implements).
- [[openai]] — closed-source counterpart.
- [[orchestration]] — orchestration framework landscape.
- [[coding_agents]] — coding-agent landscape (smolagents can be used here).
- [[../frameworks/langgraph|langgraph]] (P5.AB) — provider-neutral graph orchestration.
- [[../frameworks/dspy|dspy]] (P5.AB) — prompt-and-weight compiler complement.
- [[../mcp/README|mcp]] — Model Context Protocol; smolagents speaks MCP for tools.
- [[../llm/runtimes/ollama|ollama]] (P5.AD) — common local LLM substrate.
- [[../llm/models/llama|llama]] (P5.AD) — model family commonly used.

## Keywords

`#smolagents` `#huggingface` `#code-agents` `#sandboxed` `#e2b` `#lightweight`

## References / raw notes

- Docs: <https://huggingface.co/docs/smolagents/>
- Repo: <https://github.com/huggingface/smolagents>
- Launch blog (Dec 2024): <https://huggingface.co/blog/smolagents>
- HF Agents Course code-agents unit: <https://huggingface.co/learn/agents-course/unit2/smolagents/code_agents>
- E2B (recommended secure-sandbox provider): <https://e2b.dev/>

### Original raw notes

> **smolagents** is a library that enables you to run powerful agents in a few lines of code. It offers:
>
> - ✨ **Simplicity**: the logic for agents fits in ~1,000 lines of code (see `agents.py`). We kept abstractions to their minimal shape above raw code!
> - 🧑‍💻 **First-class support for Code Agents.** Our `CodeAgent` writes its actions in code (as opposed to "agents being used to write code"). To make it secure, we support executing in sandboxed environments via E2B or via Docker.
> - 🤗 **Hub integrations:** you can share/pull tools or agents to/from the Hub for instant sharing of the most efficient agents.
> - 🌐 **Model-agnostic:** smolagents supports any LLM. It can be a local `transformers` or Ollama model, one of many providers on the Hub, or any model from OpenAI, Anthropic and many others via LiteLLM integration.
> - 👁️ **Modality-agnostic:** Agents support text, vision, video, even audio inputs (cf. the vision tutorial).
> - 🛠️ **Tool-agnostic:** you can use tools from LangChain, MCP, or even a Hub Space as a tool.
