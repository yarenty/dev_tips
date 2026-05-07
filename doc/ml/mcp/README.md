---
title: MCP — Model Context Protocol
keywords: [mcp, anthropic, agents, tools, resources, protocol]
status: reviewed
review_date: 2026/05/03
---

# MCP — Model Context Protocol

> Anthropic's open standard (Nov 2024) for connecting LLM apps to data sources and tools.
> The hottest standard in the agent ecosystem 2024-2025.

## What is MCP?

**Model Context Protocol** is an open client–server protocol that gives LLM applications
a uniform way to talk to external data and capabilities. It defines three primitives:

- **Tools** — LLM-callable functions (the model decides when to invoke).
- **Resources** — readable data the host can attach to the model's context (files, DB rows, search results).
- **Prompts** — reusable templates the host or user picks from.

Plus auxiliary primitives: **sampling** (server asks the host LLM for a completion),
**roots** (filesystem scope hint), **elicitation** (server asks the user a structured
question).

## Why it matters: the M×N problem

Without a standard, integrating *M* LLM clients with *N* tools is M×N pieces of glue.
MCP collapses this into **M+N**: each LLM speaks MCP once, each tool exposes MCP once.
That's the same compression USB / LSP / OCI did for their respective ecosystems, and the
reason MCP swept the agent space within months of release.

## Adoption (mid-2025)

**Hosts that speak MCP:** Claude Desktop (first), Cursor, Goose (Block), Continue, Zed,
Windsurf, Cline, Sourcegraph Cody, JetBrains AI Assistant — by mid-2025 essentially all
major agent clients support MCP, sometimes alongside their own legacy tool format.

**Reference SDKs:** official Python / TypeScript / Rust / C# / Go / Java / Kotlin /
Swift implementations under [`modelcontextprotocol/`](https://github.com/modelcontextprotocol).
Reference servers (`servers/` repo) include filesystem, git, GitHub, GitLab, Slack,
postgres, sqlite, brave-search, fetch, memory, time, sequential-thinking, and more.

**Server registries:** [`punkpeye/awesome-mcp-servers`](https://github.com/punkpeye/awesome-mcp-servers)
is the de-facto index; vendors are starting to publish official registries.

## Articles in this section

- [[articles|MCP — curated articles & critiques]] — reading list (spec, security, A2A vs MCP).
- [[a2a|A2A — Google's Agent2Agent protocol]] — agent-to-agent counterpart (Apr 2025; LF Jun 2025).
- [[mcp4db|MCP for databases]] — Toolbox + postgres-mcp + SQL-injection-via-prompt-injection.
- [[mcp_ui|mcp-ui — interactive UI from MCP servers]] — agent-generated UI proposal.

## Where to start

| If you want to… | Start with… |
|----|----|
| Understand the protocol | The [MCP spec](https://modelcontextprotocol.io/) |
| Wire MCP into an agent CLI | Goose ([[../llm/runtimes/goose|runtimes/goose]]) — MCP-first |
| Expose a database to an LLM | [[mcp4db]] |
| Build your own server | The Python or TypeScript SDK in `modelcontextprotocol/` |
| Compare A2A vs MCP vs ACP | [[articles]] (the elisowski cheat sheet) |
| Read critiques | [[articles]] (raz.sh, timkellogg.me) |

## See also

- [[../agents/README|agents]] — agent landscape; MCP is the integration substrate.
- [[../agents/agent_instructs|agent_instructs]] — system-prompt / instruction patterns for MCP-using agents.
- [[../llm/README|llm]] — LLM hub.
- [[../llm/runtimes/goose|goose]] — MCP-native agent CLI.
- [[../llm/system_prompts/README|system_prompts]] — adjacent shaping lever.
- [[../finetuning/README|finetuning]] — sibling section.
- [[../tools/README|tools]] — broader ML tooling.

## Keywords

`#mcp` `#anthropic` `#agents` `#tools` `#resources` `#protocol`
