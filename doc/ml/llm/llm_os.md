---
title: LLM OS — Karpathy's "LLM as kernel" framing
main_link: https://twitter.com/karpathy/status/1723140519554105733
keywords: [llm-os, karpathy, agents, architecture, tool-use, system-design]
status: reviewed
review_date: 2026/05/03
---

# LLM OS — Karpathy's "LLM as kernel" framing

**Main link:** <https://twitter.com/karpathy/status/1723140519554105733>

## Summary

"LLM OS" is Andrej Karpathy's framing — popularised in a Nov 2023 tweet and the *Intro to LLMs* / *State of GPT* talks — that puts the LLM in the role of a CPU/kernel inside an emerging operating-system architecture. Around the kernel sit "syscalls": tools (calculator, code interpreter, browser), memory (context window + RAG + persistent stores), peripherals (vision, audio, video I/O), other LLMs (multi-agent), and a "system 2" deliberation loop. The phidata/agno team published a [reference cookbook implementation](https://github.com/agno-agi/agno) of the idea so you can play with the pattern concretely. The framing has since become the implicit architecture for most production agent systems.

## Insight

The LLM-OS framing is most useful as a **mental model** for designing agent systems: when you're stuck deciding "should this be a tool, a sub-agent, a piece of context, or memory?" the OS analogy gives you a vocabulary. The kernel/syscall split also predicts where the bottlenecks land — the kernel (model) is slow and stateless, so amortise its cost via caching (KV / prompt / semantic — see [[../memory/caching|memory/caching]]), batch the syscalls, and push as much logic as possible into deterministic tools rather than back into model tokens.

The framing has competition: **LangChain's chain abstraction** (composable runnables; weaker on agency), **AutoGen's multi-agent conversation** (model-as-actor in a society), **DSPy's "language-model program"** (compiled prompts/weights, see [[../frameworks/dspy|dspy]]), and Anthropic's recent **"computer use" + skills** angle. None of these are mutually exclusive — the LLM-OS view is the most architectural; the others are programming-model takes that fit inside it. In 2025 the convergence is clear: MCP (see [[../mcp/README|mcp]]) is becoming the standard "syscall ABI", and agent frameworks (LangGraph, agno/phidata, smolagents) are the userland.

## Similar / related topics

- Andrej Karpathy's *Intro to LLMs* talk (Nov 2023) and *State of GPT* (May 2023) — the talks that crystallised the framing.
- LangGraph — graph-based stateful agent orchestration; the practical "userland" for LLM-OS-shaped systems. See [[../frameworks/langgraph|langgraph]].
- agno (formerly phidata) — agent + RAG framework; ships the original LLM-OS cookbook. See [[../frameworks/phidata|phidata]].
- MCP (Model Context Protocol) — the de-facto "syscall ABI" for LLMs to talk to tools/resources. See [[../mcp/README|mcp]].
- Voyager / Generative Agents / Stanford Smallville — earlier research demos that prefigured the LLM-OS pattern.

## Internal links
- [[README|llm]]
- [[../mcp/README|mcp]]
- [[../agents/README|agents]]
- [[../memory/README|memory]]
- [[../rag/README|rag]]
- [[../frameworks/langgraph|frameworks/langgraph]]
- [[../frameworks/phidata|frameworks/phidata]]
- [[../frameworks/dspy|frameworks/dspy]]

## Keywords

`#llm-os` `#karpathy` `#agents` `#architecture` `#tool-use` `#system-design`

## References / raw notes

### Karpathy's framing

The seed tweet: <https://twitter.com/karpathy/status/1723140519554105733>. Re-explored in:

- *Intro to Large Language Models* (Nov 2023): <https://www.youtube.com/watch?v=zjkBMFhNj_g>
- *State of GPT* (May 2023, Microsoft Build): <https://www.youtube.com/watch?v=bZQun8Y4L2A>

### The LLM OS philosophy

- LLMs are the **kernel process** of an emerging operating system.
- This kernel solves problems by coordinating other resources (memory, tools, peripherals, other LLMs).
- The vision (Karpathy's checklist):
  - [x] Read/generate text.
  - [x] Has more knowledge than any single human about most subjects.
  - [x] Can browse the internet.
  - [~] Can use existing software infrastructure (calculator, Python, mouse/keyboard) — partial in 2024, mainstream by 2025 via tool use + computer-use APIs.
  - [~] Can see and generate images and video — partial via multimodal models (GPT-4o, Claude 3.5 Sonnet, Gemini 1.5/2.0).
  - [~] Can hear and speak, generate music — partial (Whisper, OpenAI Realtime, Suno).
  - [~] Can think for a long time using a "system 2" — emerging via o1/o3, DeepSeek-R1, QwQ test-time compute.
  - [ ] Can self-improve in domains.
  - [ ] Can be customized and fine-tuned for specific tasks — possible in principle ([[../finetuning/README|finetuning]]) but not "OS-level".
  - [x] Can communicate with other LLMs.

### Reference implementation — agno (formerly phidata) cookbook

<https://github.com/agno-agi/agno> (the rebranded phidata) — the LLM-OS cookbook lives in the same repo and gives a concrete implementation of the pattern: an LLM kernel orchestrating web search, calculator, Python interpreter, file system, and other LLM "agents".

Originally at <https://github.com/phidatahq/phidata/tree/main/cookbook/llm_os>; the org rebranded in late 2024.
