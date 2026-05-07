---
title: HuggingFace agent tooling — smolagents tutorials & agent course
main_link: https://huggingface.co/docs/smolagents/tutorials/building_good_agents
keywords: [huggingface, hf, smolagents, agents-course, tutorials]
status: reviewed
---

# HuggingFace agent tooling

**Main link:** <https://huggingface.co/docs/smolagents/tutorials/building_good_agents>

## Summary

This article is the **HuggingFace agent-documentation pointer**: their *Building good agents* tutorial in the [[smolagents]] docs and their *Agents Course* unit on code-agents. It's the on-ramp for HF's agent stack — most of the substance lives in the [[smolagents]] article (which is HF's actively-maintained agent library), but the docs and course give you the broader narrative including HF's positioning vs LangChain/LangGraph, the *code-agents* paradigm (LLM emits Python rather than JSON tool calls), and HF's "agents are still agents" pragmatic view.

## Insight

Two distinct HF threads worth keeping straight:

1. **Old** *Transformers Agents* (May 2023) — the pre-ChatGPT-tooling era's HF attempt at agents, built on top of `transformers`. Largely deprecated; replaced by smolagents.
2. **Current** **smolagents** (Dec 2024) + **Agents Course** — the actively-maintained library and the free educational track that goes with it. The *Building good agents* doc is the practical-patterns tutorial; the Agents Course unit on code-agents is the conceptual on-ramp.

If you want the library article, go to [[smolagents]]. If you're starting from zero, the Agents Course unit (free, ~hour-long) is the best free walk-through of the *why* and the code-agents idea; *Building good agents* is the practical follow-up.

**HF's distinctive take.** The HF agents stack pushes the **code-agents pattern** (LLM emits Python that gets executed in a sandbox) over the JSON-tool-calling default that OpenAI / Anthropic SDKs use. Their argument: code expresses control flow naturally (loops, conditionals, composition), is what LLMs are most fluent in, and avoids the JSON brittleness. The trade-off is sandboxing complexity (you need E2B / Docker) and the security perimeter is larger.

**When to start here:** you're using or evaluating HF tooling, you want a free educational on-ramp, or you want HF's *code-agents* framing as a counterpoint to OpenAI/Anthropic-flavoured tutorials. Otherwise jump straight to [[smolagents]] for the library.

## Similar / related topics

- **smolagents** — the HF agent library; see [[smolagents]].
- **LangChain Academy** — the LangChain equivalent free course.
- **DeepLearning.AI agent courses** — Andrew Ng's short courses on LangGraph / AutoGen / CrewAI / etc.
- **Anthropic's *Building effective agents*** — see [[claude]].
- **HF Agents Course** — full free course at <https://huggingface.co/learn/agents-course>.

## Internal links

<!-- reviewed -->
- [[README]] — agents section landing.
- [[smolagents]] — the actively-maintained HF agent library.
- [[claude]] — Anthropic's design framing (the conceptual sibling).
- [[agent_instructs]] — `.cursorrules` / `AGENTS.md` per-repo conventions.
- [[../llm/runtimes/README|llm/runtimes]] (P5.AD) — for HF Inference Endpoints / TGI on the runtime side.

## Keywords

`#huggingface` `#hf` `#smolagents` `#agents-course` `#tutorials`

## References / raw notes

- Building good agents (smolagents tutorial): <https://huggingface.co/docs/smolagents/tutorials/building_good_agents>
- HF Agents Course — code-agents unit: <https://huggingface.co/learn/agents-course/unit2/smolagents/code_agents>
- Full HF Agents Course: <https://huggingface.co/learn/agents-course>
- The deprecated *Transformers Agents* (May 2023): <https://huggingface.co/docs/transformers/main/transformers_agents> — historical context only.
- The smolagents library (Dec 2024): <https://github.com/huggingface/smolagents>
