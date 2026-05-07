---
title: MCP — curated articles & critiques
main_link: https://modelcontextprotocol.io/
keywords: [mcp, anthropic, agents, security, prompt-injection, reading-list]
status: reviewed
---

# MCP — curated articles & critiques

**Main link:** <https://modelcontextprotocol.io/>

## Summary

A curated reading list around the **Model Context Protocol** (MCP, Anthropic, Nov 2024): the
spec & official docs, security/critique pieces, the integration-with-LangChain/LangGraph
tutorials that flooded Medium in 2025, and the cross-protocol comparisons (A2A / ACP / MCP).
Kept short on purpose — most "MCP servers you must try" listicles are link-rot bait;
the canonical entry points below stay current.

## Insight

If you're new to MCP, read **Anthropic's spec + intro post first**, then a critical piece
(`raz.sh` or `timkellogg.me` below) to get a balanced picture, then `awesome-mcp-servers`
for what's actually deployed. Skip the "10 craziest MCP servers" Medium genre — it ages
in weeks. The Microsoft `lost_in_conversation` paper that the auto-stub originally
pointed at as `main_link` is **not actually about MCP** (it's about LLMs degrading over
multi-turn conversations); it's kept below as a tangentially-relevant pointer.

## Similar / related topics

- [Anthropic intro post (Nov 2024)](https://www.anthropic.com/news/model-context-protocol) — the original announcement.
- [`modelcontextprotocol/servers`](https://github.com/modelcontextprotocol/servers) — official reference servers (filesystem, git, postgres, slack, …).
- [`punkpeye/awesome-mcp-servers`](https://github.com/punkpeye/awesome-mcp-servers) — community curation.
- A2A / ACP — sibling agent-protocols (see [[a2a]]).
- LangChain / LangGraph — the orchestration layer most MCP tutorials wrap around (see [[langgraph]]).

## Internal links
<!-- reviewed -->
- [[README]] — section landing.
- [[a2a]] — Google's agent-to-agent counterpart.
- [[mcp4db]] — MCP for databases.
- [[mcp_ui]] — MCP for interactive UI.
- [[../llm/runtimes/goose|goose]] — first OSS MCP-native agent CLI.
- [[../llm/runtimes/extensions|goose extensions]] — Goose's extensions = MCP servers.
- [[../agents/README|agents]] — agent landscape.
- [[../../programming/rust/core/reflection|reflection]] — was `reflection_step_by_step_article`, merged.

## Keywords

`#mcp` `#anthropic` `#agents` `#security` `#prompt-injection` `#reading-list`

## References / raw notes

### Spec & official

- <https://modelcontextprotocol.io/> — spec landing.
- <https://www.anthropic.com/news/model-context-protocol> — Anthropic's announcement (Nov 2024).
- <https://github.com/modelcontextprotocol/servers> — official reference servers.
- <https://github.com/punkpeye/awesome-mcp-servers> — community list.

### Tutorials (LangChain / LangGraph / Ollama wraps)

- <https://medium.com/data-science-collective/langgraph-mcp-ollama-the-key-to-powerful-agentic-ai-e1881f43cf63>
- <https://medium.com/data-science-collective/langchain-mcp-rag-ollama-the-key-to-powerful-agentic-ai-91529b2fa320>
- <https://medium.com/everyday-ai/craziest-mcp-servers-you-must-try-f23526a165f5> — listicle, ages fast.

### Critiques & security

- <https://timkellogg.me/blog/2025/04/27/mcp-is-unnecessary> — clickbait title; actual argument is "you can do most of MCP with structured outputs + a tool registry".
- <https://raz.sh/blog/2025-05-02_a_critical_look_at_mcp> — sober look at the auth/transport/security gaps in the early spec.
- <https://iamcharliegraham.substack.com/p/mcps-gatekeepers-and-the-future-of> — on who controls the MCP server registry.

### Cross-protocol comparisons

- <https://medium.com/@elisowski/what-every-ai-engineer-should-know-about-a2a-mcp-acp-8335a210a742> — A2A vs MCP vs ACP cheat sheet.

### Tangentially relevant

- <https://github.com/microsoft/lost_in_conversation> — Microsoft paper on multi-turn LLM degradation. Not about MCP per se, but motivates the "stateful tools/resources" angle.
