---
title: Goose extensions, sessions, and hints
main_link: https://block.github.io/goose/docs/getting-started/using-extensions/
keywords: [goose, extensions, mcp, sessions, goosehints, plugins]
status: reviewed
---

# Goose extensions, sessions, and hints

**Main link:** <https://block.github.io/goose/docs/getting-started/using-extensions/>

## Summary

How to extend a Goose agent: extensions (the plugin shape — built-in
ones plus arbitrary MCP servers), sessions (persistent multi-turn
conversations you can resume by name), and `.goosehints` (per-repo
or per-project system-prompt overrides). The extension model is
MCP-first, so any of the growing MCP-server ecosystem (filesystem,
GitHub, Postgres, Slack, browser-use, etc.) drops in without
Goose-specific glue.

## Insight

This is the surface area you actually *use* day-to-day. Three
patterns to know:

- **Extensions = MCP servers + a small bundled set.** The Goose
  built-ins (developer, computer-use, JetBrains, GitHub, memory)
  are the ones you'll enable first; everything else is "add an MCP
  server URL or stdio command." That makes Goose's plugin story
  identical to Claude Desktop's and Cursor's, by design.
- **Sessions are named, resumable, and cheap to fork.** Use
  `goose session --name <task>` so you can `goose session resume`
  later; great for multi-day refactors. Watch token cost on long
  sessions with closed-provider models.
- **`.goosehints` is the agent equivalent of `.cursorrules` / `AGENTS.md` /
  `CLAUDE.md`.** Drop a markdown file in your repo with house style,
  build commands, "don't touch these directories" — Goose loads it
  into the system prompt automatically.

When something feels missing (Linear, Jira, Sentry, your internal
service) the answer is almost always "find or write an MCP server,"
not "patch Goose."

## Similar / related topics

- Claude Desktop / Cursor MCP — the same plugin shape from other vendors.
- VS Code Extensions API — the IDE-extension predecessor, much heavier.
- `.cursorrules` / `AGENTS.md` / `CLAUDE.md` — sibling per-repo prompt overrides.
- LangChain Tools / function-calling — the pre-MCP plugin shape.

## Internal links
- [[goose]] — the agent runtime these features extend.
- [[providers]] — sister concept; LLM provider plugin shape.
- [[../../mcp/README|mcp]] — Model Context Protocol; how extensions are spoken.
- [[../system_prompts/system_prompt|system_prompt]] — `.goosehints` is a system-prompt override.

## Keywords

`#goose` `#extensions` `#mcp` `#sessions` `#goosehints` `#plugins`

## References / raw notes

### Extensions

- <https://block.github.io/goose/docs/getting-started/using-extensions/>

### Managing sessions

- <https://block.github.io/goose/docs/guides/managing-goose-sessions/>

### Hints / `.goosehints`

- <https://block.github.io/goose/docs/guides/using-goosehints>
