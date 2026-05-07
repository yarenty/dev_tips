---
title: skills.sh — installable agent skills
main_link: https://skills.sh/
keywords: [skills-sh, skills, agent, llm, mcp, prompt, planning, procedural-knowledge]
status: reviewed
review_date: 2026/05/03
---

# skills.sh — installable agent skills

**Main link:** <https://skills.sh/>

## Summary

`skills.sh` (by Othman Adi) is a small registry + install tool that packages **reusable agent capabilities** ("skills") as install-with-one-command bundles you can drop into an AI agent's working environment. The pitch: agents need *procedural knowledge* (how-to-do-things), not just facts; skills are the unit of distribution. The flagship example is *planning-with-files* (a structured planning skill that uses files as scratchpad). Frame this as an early experiment in the same problem space that **Claude Skills** (Anthropic, 2025) and **MCP servers** address from different angles.

## Insight

When this is interesting: you want to give an agent (Claude Code, Cursor, Goose, Devin-style coding agents) a reusable, version-controlled bundle of **prompts + tool definitions + scratch-file conventions** for a specific workflow (planning, code review, research synthesis, content production). The mental model is closer to **shell aliases / dotfiles for agents** than to a full plugin system.

When it's not the right tool: anything you'd reach for **MCP** for (a server-side tool that the agent can call over a protocol — Anthropic's now-de-facto standard) — those are a different layer; MCP gives the agent a tool, skills.sh gives the agent a how-to-think prompt + file convention. They compose.

The honest framing: this whole space is **early and fragmenting fast** in late 2024 / 2025. Anthropic has shipped first-party **Claude Skills** (released as part of Claude Code / Projects); GitHub has its **GitHub Skills** product; MCP has overtaken the conversation as the cross-vendor standard for *tool* extensibility. `skills.sh` is one bottom-up community attempt at the same problem; durable enough to bookmark, but expect the canonical answer to be Anthropic's first-party Claude Skills + MCP servers within a year or two for most workflows.

The relationship to MCP: **MCP defines the protocol** (how an agent talks to an external tool/resource server), **skills.sh defines content** (a bundle of prompts + file layouts + tool-use patterns). An MCP server *exposes* tools; a skill *trains* an agent to use them in a structured way. Many real workflows want both.

## Similar / related topics

- **Claude Skills** (Anthropic, 2025) — first-party agent-skill packaging shipped with Claude Code / Projects.
- **GitHub Skills** — different thing; GitHub's *user* learning platform but adjacent in the agent-skills conversation.
- **MCP (Model Context Protocol)** — the cross-vendor protocol for *tool / resource* extensibility; complements skills.
- **Cursor / Continue rules** — IDE-level "instructions for the agent" files (`.cursorrules`, `.continuerc`).
- **AGENT.md / CLAUDE.md / `.aider.conf.yml`** — repo-level instructions; the lightest-weight version of a skill.
- **LangChain / LangGraph templates** — heavier-weight reusable agent workflows.

## Internal links

- [[README]] — section landing.
- [[../mcp/README|mcp]] — Model Context Protocol section landing (the *tool* side of agent extensibility).
- [[../agents/README|agents]] — agentic AI section (where the skills get used).
- [[../llm/system_prompts/system_prompt|system_prompt]] — adjacent: system-prompt-as-instructions.

## Keywords

`#skills-sh` `#skills` `#agent` `#llm` `#mcp` `#prompt` `#planning` `#procedural-knowledge`

## References / raw notes

- Project: <https://skills.sh/>
- Author: Othman Adi (`othmanadi`).
- Flagship example skill: <https://skills.sh/othmanadi/planning-with-files/planning-with-files>.

### How it works (from the docs)

> Skills are reusable capabilities for AI agents. Install them with a single command to enhance your agents with access to procedural knowledge.

Install pattern:

```sh
npx skills add <owner/repo>
```

The skill is then available in the agent's working environment as a prompt + file convention bundle.

### Original tags from notes

`#skills` `#mcp` `#agent` `#llm` `#prompt` — keep these as the original author's tag set; they capture the orientation well (skills sit at the intersection of MCP-style tool extensibility, prompt engineering, and agent-runtime behaviour).

### Adjacent in the 2024-2025 landscape

- **Anthropic Claude Skills** — first-party skill packaging in Claude.ai / Claude Code / Projects (2025).
- **OpenAI Custom GPTs** — earlier (2023) version of the same idea; less code-shaped.
- **MCP servers ecosystem** — <https://github.com/modelcontextprotocol/servers> — the canonical list.
- **awesome-claude-code** / similar lists for skill-shaped patterns in agent toolkits.
