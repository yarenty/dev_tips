---
title: Goose — Block's open-source AI agent runtime
main_link: https://block.github.io/goose/
keywords: [goose, agent, mcp, block, cli, deepseek, extensions]
status: reviewed
---

# Goose — Block's open-source AI agent runtime

**Main link:** <https://block.github.io/goose/>

## Summary

Goose is an open-source, extensible AI agent built and sponsored by
Block (Square/Cash App), distributed as a CLI plus desktop app. It
runs locally, plugs into any LLM provider (OpenAI, Anthropic, Ollama,
Bedrock, Google, etc.), and gains capabilities by attaching MCP servers
or built-in extensions (developer tools, computer-use, GitHub, JetBrains,
shell). Recipes/sessions/hints give it persistence and reproducibility.
The agent's loop is the standard plan-act-observe-tool-call cycle —
the differentiator is the open architecture and the Block-backed
contributor base.

## Insight

Position Goose as the **open, vendor-neutral alternative to the
closed agent CLIs**: Claude Code, Cursor, OpenAI Codex CLI, GitHub
Copilot Workspace, Devin. It's closer in spirit to Aider and Cline
(open-source, BYO-model) than to Cursor (proprietary, model-lock).
What you actually get over those:

- **MCP-first.** Goose was an early adopter of the Model Context
  Protocol, so it composes cleanly with the growing MCP-server
  ecosystem rather than inventing a private plugin shape.
- **Provider-agnostic.** Switch between Claude, GPT-4o, DeepSeek-R1,
  and a local Ollama model without changing the agent. See
  [[providers]] for the supported list.
- **Extensions, recipes, hints.** [[extensions]] for tool plugins,
  recipes for parameterised replayable workflows, `.goosehints` for
  per-repo system-prompt overrides.

The honest gotchas: it's still rougher around the edges than Claude
Code or Cursor; sessions can be expensive on closed providers; for
real autonomy on long tasks you want a strong reasoning model
(R1, Claude Sonnet 4, GPT-5-class) rather than the local 8B that
the demo videos use.

## Similar / related topics

- Claude Code — Anthropic's official terminal coding agent (closed).
- Cursor — IDE-shaped AI coding agent (closed, model-multiplexed).
- Cline / Continue.dev — open-source IDE-side agents.
- Aider — git-aware terminal coding agent; predates the MCP wave.
- OpenAI Codex CLI — OpenAI's agent CLI; tighter coupling to GPT-5.
- Devin (Cognition) — fully autonomous SWE agent; closed, hosted.

## Internal links
- [[extensions]] — Goose's plugin/extension system.
- [[providers]] — supported LLM providers (Ollama, Anthropic, OpenAI, …).
- [[ollama]] — the most common local backend.
- [[../models/deepseek|models/deepseek]] — DeepSeek-R1 (the `michaelneale/deepseek-r1-goose` tool-calling variant).
- [[../../mcp/README|mcp]] — Model Context Protocol; how Goose talks to tools.
- [[../../agents/README|agents]] — broader agent-framework landscape.

## Keywords

`#goose` `#agent` `#mcp` `#block` `#cli` `#deepseek` `#extensions`

## References / raw notes

- Site: <https://block.github.io/goose/>
- Repo: <https://github.com/block/goose>

### Pitch (from the site)

- **Open source** — built with transparency and collaboration in mind;
  contributors can customise and innovate freely.
- **Runs locally** — executes tasks on your machine, keeping control
  in your hands.
- **Extensible** — pair with any LLM and connect to any external MCP
  server or API.
- **Autonomous** — can independently handle multi-step tasks
  (debugging, deployment, refactors).

> An open-source, extensible AI agent that goes beyond code suggestions —
> install, execute, edit, and test with any LLM.

### Sub-articles auto-split from this file

- [[providers]] — supported LLM providers.
- [[extensions]] — extensions / plugins.
