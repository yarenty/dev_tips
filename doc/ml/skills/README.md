---
title: Skills — agent-skill packaging
keywords: [skills, agent, mcp, claude-skills, procedural-knowledge]
status: reviewed
review_date: 2026/05/03
---

# Skills — agent-skill packaging

A small section about **agent skills** — the unit of distribution for *procedural knowledge* (how to do a task, in a structured way) given to an AI agent. The space is early and fragmenting in 2024-2025: bottom-up community efforts (`skills.sh`), first-party platform offerings (Anthropic Claude Skills, OpenAI Custom GPTs), and the orthogonal **MCP** standard for *tool* extensibility.

## Articles

- [skills.sh — installable agent skills](skills.sh.md) — Othman Adi's `npx skills add ...` registry; flagship example *planning-with-files*. Frame as one early bottom-up attempt in a space that's converging toward Claude Skills + MCP.

## The 2024-2025 picture

Three distinct things often conflated under "agent skills":

1. **Prompts + workflows** as a packaged unit — *what skills.sh ships.* Closer to dotfiles / shell-aliases for agents.
2. **Tools / resources** the agent can call — *what MCP servers expose.* The cross-vendor protocol for this is now Anthropic's MCP (introduced late 2024, broadly adopted by 2025).
3. **First-party skill packaging** — *what Claude Skills, Custom GPTs, etc. provide.* Often combine #1 and #2 into a vendor-shaped bundle.

For most production work in 2025, the answer is some combination of MCP servers (for tools) + a `CLAUDE.md` / `AGENT.md` / `.cursorrules` file (for prompts/workflows) + first-party skill packaging where it exists. `skills.sh`-style community registries fit *between* these.

## Keywords

`#skills` `#agent` `#mcp` `#claude-skills` `#procedural-knowledge` `#agent-tools`

## See also

- [[../mcp/README|mcp]] — Model Context Protocol section (the *tool* axis).
- [[../agents/README|agents]] — agentic AI section (where skills get used).
- [[../llm/system_prompts/system_prompt|system_prompt]] — system-prompt-as-instructions; lightest-weight version of a skill.
- [[../README]] — ML section landing.
