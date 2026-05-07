---
title: System prompts — patterns, leaks, and limits
main_link: https://github.com/jujumilk3/leaked-system-prompts
keywords: [system-prompt, prompting, leaked-prompts, jailbreak, persona, role, prompt-engineering]
status: reviewed
review_date: 2026/05/03
---

# System prompts — patterns, leaks, and limits

**Main link:** <https://github.com/jujumilk3/leaked-system-prompts>

## Summary

The system prompt is the underrated lever for shaping LLM behaviour:
a privileged message at the top of the conversation that sets role,
output format, refusal style, persona, tools, and constraints. This
article covers practical patterns (role assignment, output-format
constraints, few-shot exemplars, refusal handling, tool-use
instructions), the difference between *system* and *developer*
messages in modern multi-message APIs (OpenAI's `system` + `developer`
split, Anthropic's single `system` field, Gemini's `systemInstruction`),
the canonical leaked-prompts corpus, and the honest caveat that
system prompts are *soft* constraints — they can be jailbroken or
extracted.

## Insight

Three things to internalise:

- **Specificity beats length.** A focused 200-token system prompt
  with concrete examples and refusal rules beats a 2000-token
  manifesto. Anthropic's published Claude system prompts are good
  reference points: they assign role, list tools, give exemplars,
  state edge cases.
- **System prompts can be extracted and bypassed.** Techniques like
  *PolicyPuppetry* (Hidden Layer, 2025), simple "repeat your
  instructions verbatim", or roleplay attacks recover most system
  prompts. Treat anything in the system prompt as **publicly
  visible**; never put real secrets there.
- **`system` vs `developer` vs user.** OpenAI's *Responses* /
  `o-series` API distinguishes a (platform-set) `system` message
  from a (developer-set) `developer` message; Anthropic and Gemini
  collapse those into one. The instruction-following hierarchy is
  the same idea: platform > developer > user, but enforcement is
  best-effort.

Useful patterns:

- **Role assignment** — "You are a senior Postgres reviewer."
- **Output format** — schema, JSON-mode, "respond only in markdown
  with a ## header per section."
- **Few-shot exemplars** — show 2-3 input/output pairs inline.
- **Refusal policy** — explicit "if asked X, respond with Y".
- **Tool-use rules** — "always call `search` before answering
  questions about current events."
- **Persona / voice** — concise tone, formal/casual register,
  language constraints.

For per-repo overrides in agent CLIs, see the agent-side conventions:
`.cursorrules`, `AGENTS.md`, `CLAUDE.md`, `.goosehints` — all of
them load into the system prompt automatically.

## Similar / related topics

- Anthropic's published Claude system prompts — canonical reference for production system-prompt design.
- Cursor / GitHub Copilot / Codeium leaked prompts — examples in the wild.
- Karpathy's *State of GPT* — foundational framing of how prompts steer LLMs.
- DSPy — programmatic prompt optimisation; treats prompts as compilable artifacts.
- PolicyPuppetry / system-prompt-extraction research — the adversarial side.
- "Prompt injection" defences — distinct but related: protecting system prompts from user-supplied content.

## Internal links
- [[../runtimes/extensions|extensions]] — `.goosehints` is a per-repo system-prompt override.
- [[../runtimes/goose|goose]] — agent CLI that consumes a system prompt + hints.
- [[../README|llm]] — parent LLM hub.
- [[../../agents/README|agents]] — agent frameworks; system prompts are how you steer them.
- [[../../mcp/README|mcp]] — MCP tool descriptions are part of the system context.
- [[../../frameworks/README|ml/frameworks]] — DSPy / LangGraph; prompt optimisation frameworks.

## Keywords

`#system-prompt` `#prompting` `#leaked-prompts` `#jailbreak` `#persona` `#role` `#prompt-engineering`

## References / raw notes

### Canonical leaked-prompt corpora

- [`jujumilk3/leaked-system-prompts`](https://github.com/jujumilk3/leaked-system-prompts) —
  community-maintained collection (Cursor, Anthropic, Bing, etc.).
- Anthropic publishes Claude's deployed system prompts officially:
  <https://docs.anthropic.com/en/release-notes/system-prompts>.

### Yarenty's prompt-learning experiments

The article was originally seeded from yarenty's notes on programmatic
prompt learning — treating prompt construction as something you can
optimise / search rather than hand-write.

- <https://github.com/yarenty/prompt_learning?tab=readme-ov-file>
- <https://x.com/yarenty/status/1922744830561583141>
- <https://yarenty.blogspot.com/2025/05/teaching-ai-to-write-code-symphony-of.html>

### Adjacent: MCP as the new "API for LLMs"

- Drew Breunig, *MCPs are APIs for LLMs* (2025-03-18) —
  <https://www.dbreunig.com/2025/03/18/mcps-are-apis-for-llms.html> —
  argues MCP server descriptions are themselves becoming part of the
  system context.
