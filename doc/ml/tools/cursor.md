---
title: Cursor — Anysphere's AI-first IDE
main_link: https://cursor.com/
keywords: [cursor, anysphere, ai-ide, copilot, composer, claude, gpt, coding-agent]
status: reviewed
review_date: 2026/05/03
---

# Cursor — Anysphere's AI-first IDE

**Main link:** <https://cursor.com/>

## Summary
Cursor is Anysphere's AI-first IDE: a fork of VS Code with deep, native integration of LLM-driven editing. The signature surfaces are **Tab** (next-edit autocomplete trained on real edit sequences), **Chat** (a side-panel where you can talk to the codebase), **Composer / Agent** (multi-file edits across the project, optionally autonomous over multiple turns), **Bug Finder** (CI-style scan for issues), and per-repo customisation through `.cursorrules` (legacy) or `AGENTS.md` / `CLAUDE.md`-style instruction files.

## Insight
Reach for Cursor when you want a **stateful, codebase-aware coding agent welded into your editor** rather than the over-the-shoulder autocomplete of GitHub Copilot. The practical workflow people actually find sticky: draft the *plan* in ChatGPT or Claude (longer context, better at high-level design), drop the plan into Cursor as a `.cursorrules` / `AGENTS.md` snippet (test-first, no random refactors, etc.), then let Composer execute. `.cursorignore` keeps junk and secrets out of the indexed context.

Vs the rest of the agentic-IDE landscape:
- **vs Claude Code / Codex CLI** — terminal-native; better when you live in tmux, worse when you want inline diffs.
- **vs Codeium / Windsurf (now Codeium's IDE)** — Windsurf is the closest direct competitor on the "VS Code fork + agent" axis; pricing and model availability differ.
- **vs Continue.dev** — open-source, BYO-model VS Code extension; less polished but no vendor lock-in.
- **vs GitHub Copilot** — Copilot's strength is the autocomplete reflex; Cursor's strength is the multi-file Composer flow.
- **vs [[../agents/coding_agents|coding_agents]] more broadly** — Cursor sits in the "IDE chat + agent" form factor cell. See that landscape article for the full taxonomy.

The 2024–2025 pricing reality matters: Cursor Pro at $20/mo, with cap-based rate limiting on the strongest models; Cursor Pro+ as a higher tier; the controversial September 2025 pricing change moved Pro from "500 fast requests" to a usage-based credit model that surprised a lot of long-time users. Quality of the Tab model is the under-discussed differentiator — it's trained on real Cursor-user edit sessions, so it predicts your *next change* rather than just the next token. Privacy mode is real but you're still sending source to Anysphere's inference path; for regulated environments [[tabby]] or self-hosted Continue.dev are saner.

## Similar / related topics
- [[../agents/coding_agents|coding_agents]] — the broader landscape (Cursor / Aider / Claude Code / Codex / Cline / Continue / Cody / Windsurf / etc.) with a form-factor picker table.
- [[../agents/claude|claude]] — Anthropic's *Building effective agents* engineering reference; useful mental model for what Composer is doing.
- [[../agents/agent_instructs|agent_instructs]] — `.cursorrules` / `AGENTS.md` / `CLAUDE.md` / `.goosehints` per-repo override convention.
- [[tabby]] — self-hosted, open-source code-completion alternative for regulated environments.
- [Codeium / Windsurf](https://codeium.com/windsurf) — direct competitor; same VS-Code-fork-with-agent shape.

## Internal links
- [[../agents/coding_agents|coding_agents]]
- [[../agents/agent_instructs|agent_instructs]]
- [[../agents/claude|claude]]
- [[tabby]]

## Keywords
`#cursor` `#anysphere` `#ai-ide` `#composer` `#tab` `#coding-agent`

## References / raw notes

Practical workflow tips that have aged well:

- Start the plan in ChatGPT or Claude (longer context, better at architecture); paste it into Cursor as a Composer prompt or pin as an `AGENTS.md` / `.cursorrules` file.
- `.cursorrules` (legacy) / `AGENTS.md` / `CLAUDE.md`: test-first prompt, "no unrelated refactors", style notes, project conventions.
- `.cursorignore`: exclude `node_modules/`, build artefacts, secrets, generated code, large vendored deps. Keeps the indexed context relevant and reduces noise/cost.
