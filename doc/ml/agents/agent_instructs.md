---
title: Agent instructions — `.cursorrules` / `AGENTS.md` / `CLAUDE.md` / `.goosehints`
main_link: https://agents.md/
keywords: [agent-instructions, cursorrules, agents-md, claude-md, goosehints, system-prompts]
status: reviewed
---

# Agent instructions — `.cursorrules` / `AGENTS.md` / `CLAUDE.md` / `.goosehints`

**Main link:** <https://agents.md/>

## Summary

In 2024-2025 a new convention emerged: **a markdown file checked into the repo root that the local AI coding agent reads on every session** — the per-project equivalent of a system prompt. Cursor's `.cursorrules` started it (early 2024), GitHub Copilot followed with `.github/copilot-instructions.md`, OpenAI's Codex CLI standardised on `AGENTS.md` (May 2025) which Anthropic's Claude Code mirrored as `CLAUDE.md`, and Block's [[../llm/runtimes/goose|Goose]] uses `.goosehints`. The convergence is striking: all are markdown, all live in the repo, all override the agent's defaults with project-specific style/build/test conventions. `agents.md` (the website) is now the de facto cross-vendor convention pushed by OpenAI.

## Insight

The genuine insight is that **the system prompt is a per-product surface, but the project conventions are a per-repo surface**, and conflating the two led to two years of "my agent doesn't follow our style guide" complaints. The per-repo file fixes that:

- **What goes in it.** The community has converged on roughly: build commands (`how do I run this thing`), test commands (`how do I check I haven't broken it`), code style (linters/formatters and any non-obvious house rules), dependency management (which package manager, lockfile policy), project-specific vocabulary (domain glossary if your code uses unfamiliar terms), and explicit *don'ts* (e.g. "don't add new dependencies without asking", "don't touch the migrations directory").
- **What does NOT go in it.** Secrets. Long architectural narratives the agent doesn't need. Anything that changes daily. Per-author preferences (use editor config + lint rules instead).
- **Length sweet spot.** ~50-300 lines. Above that, agents start losing the middle (the lost-in-the-middle problem). Several teams now split into a short `AGENTS.md` at the root and longer per-directory `AGENTS.md` files that the agent only reads when it enters that directory.
- **Multi-agent reality.** A repo often ends up with several of these files (`.cursorrules`, `AGENTS.md`, `CLAUDE.md`, `.goosehints`, `.windsurfrules`, `.continuerules`...). The `agents.md` initiative tries to canonicalise on `AGENTS.md` so all agents read the same file, but adoption is uneven. **Practical rule**: pick one file as the canonical source; symlink or copy the others.
- **Trust boundary.** These files are part of your codebase, so they're version-controlled, code-reviewed, and visible to collaborators — exactly the properties a system prompt should have. They're also visible to anyone who clones the repo, so don't put credentials or sensitive prompts.

**When this matters most:** large codebases with non-obvious conventions, monorepos, codebases where the agent will be doing autonomous (not just suggest-and-accept) work, and teams onboarding to AI agents where new contributors need agent + human onboarding to converge.

**Vs system prompts:** the per-product system prompt (the [[../llm/system_prompts/system_prompt|system_prompt]] article from P5.AD) sets the agent's *personality* and tools; this file sets the *project* it's working on. Both are needed. The agent receives both.

## Similar / related topics

- **`.cursorrules`** — Cursor's original (Jan 2024) per-project rules file; sometimes also `.cursor/rules/*.mdc` for multi-rule setups.
- **`AGENTS.md`** — OpenAI Codex CLI's convention (May 2025), now a cross-vendor initiative at <https://agents.md/>.
- **`CLAUDE.md`** — Anthropic Claude Code's convention; same idea, different filename. Often also a hierarchy of `CLAUDE.md` files at multiple directory levels.
- **`.github/copilot-instructions.md`** — GitHub Copilot's repo-instructions file (also supports per-language `.instructions.md`).
- **`.goosehints`** — Block Goose's equivalent; covered in [[../llm/runtimes/extensions|extensions]].
- **`.windsurfrules` / `.continuerules` / `.aider.conf.yml`** — Windsurf, Continue.dev, Aider equivalents.

## Internal links

- [[README]] — agents section landing.
- [[claude]] — Anthropic's *Building effective agents* (the broader design background).
- [[../llm/system_prompts/system_prompt|system_prompt]] (P5.AD) — system-prompt patterns and leaks (the per-product surface).
- [[../llm/system_prompts/README|system_prompts]] (P5.AD) — system-prompts section.
- [[../llm/runtimes/goose|goose]] (P5.AD) — Block's open agent CLI (uses `.goosehints`).
- [[../llm/runtimes/extensions|extensions]] (P5.AD) — Goose extensions/sessions/hints.
- [[../mcp/README|mcp]] — Model Context Protocol (the tool-side complement).
- [[devin]] — Devin / OpenHands (also reads instructions files).
- [[coding_agents]] — broader coding-agent landscape.

## Keywords

`#agent-instructions` `#cursorrules` `#agents-md` `#claude-md` `#goosehints` `#system-prompts`

## References / raw notes

- The `AGENTS.md` cross-vendor initiative: <https://agents.md/>
- Cursor docs on rules: <https://docs.cursor.com/context/rules>
- Anthropic Claude Code memory & `CLAUDE.md` docs: <https://docs.anthropic.com/en/docs/claude-code/memory>
- OpenAI Codex CLI announcement (May 2025): <https://openai.com/index/introducing-codex/>
- GitHub Copilot custom instructions: <https://docs.github.com/en/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot>
- Aider conventions: <https://aider.chat/docs/usage/conventions.html>
- Continue.dev rules: <https://docs.continue.dev/customization/rules>
- Goose hints: <https://block.github.io/goose/docs/guides/using-goosehints/>

### Practical template for an `AGENTS.md` / `CLAUDE.md`

```markdown
# Project agent instructions

## Build
- Install: `uv sync`
- Run: `uv run myapp`

## Test
- Unit: `uv run pytest -q`
- Lint: `uv run ruff check .`
- Type: `uv run mypy src/`

## Conventions
- Python 3.12+, type hints required on public APIs.
- No new top-level dependencies without discussion.
- All DB migrations go through Alembic; do not edit `migrations/versions/*` by hand.

## Don'ts
- Don't touch `infra/terraform/` — managed separately.
- Don't write to `dist/` — it's gitignored output.

## House style
- Docstrings: Google style.
- Commit messages: Conventional Commits.
```
