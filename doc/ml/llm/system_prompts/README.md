---
title: System prompts — the underrated lever for shaping LLMs
keywords: [system-prompts, prompt-engineering, leaked-prompts, persona, refusal]
status: reviewed
review_date: 2026/05/03
---

# System prompts — the underrated lever for shaping LLMs

The system-prompt landing. System prompts are the privileged
top-of-conversation messages that set role, output format, refusal
policy, persona, and tool-use rules. They sit between the model and
the user and shape behaviour more than most fine-tuning could —
yet they're soft constraints that can be extracted or jailbroken.

## Articles

- [[system_prompt]] — patterns (role, format, exemplars, refusals, tool-use), the
  `system` vs `developer` vs user message distinction, the leaked-prompts corpus,
  and the honest "system prompts can be extracted" caveat.

## Canonical references (no article yet)

- [`jujumilk3/leaked-system-prompts`](https://github.com/jujumilk3/leaked-system-prompts) —
  community corpus of leaked system prompts (Cursor, Anthropic, Bing/Sydney, …).
- [Anthropic's published Claude system prompts](https://docs.anthropic.com/en/release-notes/system-prompts) —
  the only major lab that releases its production system prompts.
- Karpathy's *State of GPT* talk — foundational framing for prompting LLMs.
- Drew Breunig, *MCPs are APIs for LLMs* — argues MCP tool descriptions are now
  part of the effective system context.

## Cross-section see-also

- [[../README|llm]] — parent LLM hub.
- [[../runtimes/extensions|runtimes/extensions]] — `.goosehints` is a per-repo
  system-prompt override; the same shape as `.cursorrules` / `AGENTS.md` / `CLAUDE.md`.
- [[../runtimes/goose|runtimes/goose]] — agent CLI that loads system prompts + hints.
- [[../../agents/README|agents]] — agent frameworks; system prompts are how you steer them.
- [[../../mcp/README|mcp]] — Model Context Protocol; tool descriptions extend the system context.
- [[../../frameworks/README|ml/frameworks]] — DSPy / LangGraph; prompt-as-program frameworks.

## Keywords

`#system-prompts` `#prompt-engineering` `#leaked-prompts` `#persona` `#refusal`
