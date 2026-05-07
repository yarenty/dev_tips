---
title: emdash — multi-agent worktree UI for coding-agent CLIs
main_link: https://github.com/generalaction/emdash
keywords: [emdash, coding-agent, worktree, multi-agent, ui]
status: reviewed
review_date: 2026/05/03
---

# emdash — multi-agent worktree UI for coding-agent CLIs

**Main link:** <https://github.com/generalaction/emdash>

## Summary

**emdash** (generalaction/emdash) is a cross-platform UI layer for **running multiple coding-agent CLIs in parallel**, each in its own git worktree, all visible from one window. It supports OpenAI Codex CLI, Claude Code, Droid (Factory CLI), Gemini CLI, Cursor CLI, Amp Code CLI, GitHub Copilot CLI, and Charm CLI. The pitch is *fan-out / compare / merge*: launch the same task at three different agents, watch them work in parallel without interfering, then keep whichever PR you like best.

## Insight

This is a category of tool that didn't exist 18 months ago and is suddenly useful because:

- **Coding-agent CLIs proliferated.** Most developers now have 2-4 of these CLIs installed (Claude Code + Codex + Cursor + Goose is a common stack); switching contexts manually is friction.
- **Git worktrees are the right primitive.** Each agent gets its own working directory pointing at the same repo and a different branch — no file-edit collisions, fully parallel, easy to compare diffs at the end.
- **The `agents.md` convention helps.** When all the agents read the same `AGENTS.md` ([[agent_instructs]]), running the same task across them is a fair comparison, not a configuration nightmare.

**When to reach for it:** the *exploration* phase of a non-trivial change, where you genuinely don't know which approach is best and you'd happily spend $5 of model compute to see three different agents' takes side-by-side. Also useful for benchmarking new agent CLIs against your existing one without disrupting your workflow.

**When NOT to reach for it:** day-to-day driving where you've already settled on one CLI; the orchestration overhead isn't worth it. Also: emdash itself is small and young; if you need this functionality at production scale (CI fan-out, batch evaluation), build it directly with `git worktree add` + your CLI of choice.

**Vs alternatives.** [Conductor](https://conductor.build/) is a similar Anthropic-affiliated tool focused specifically on Claude Code. [Cody Lab](https://sourcegraph.com/blog/lab) and a few VS Code extensions give partial parallel-agent functionality inside an IDE. emdash is the most agent-agnostic option as of late 2025.

## Similar / related topics

- **Conductor** — Anthropic-flavoured parallel Claude Code worktrees.
- **`git worktree`** — the underlying git primitive emdash leans on.
- **Aider's `--watch-files`** — single-agent, watch-mode-driven workflow.
- **Cline / Roo Code** — multi-instance VS Code agents (different shape; in-IDE rather than per-worktree).
- **emdash's own roadmap** — they've talked about MCP-based extension points; worth tracking.

## Internal links

- [[README]] — agents section landing.
- [[coding_agents]] — landscape of the CLIs emdash wraps.
- [[agent_instructs]] — `AGENTS.md` / `CLAUDE.md` / `.cursorrules` (what each subordinate agent reads).
- [[devin]] — Devin / OpenHands (the autonomous-bot side, complementary).
- [[../llm/runtimes/goose|goose]] (P5.AD) — open vendor-neutral CLI.

## Keywords

`#emdash` `#coding-agent` `#worktree` `#multi-agent` `#ui`

## References / raw notes

- Repo: <https://github.com/generalaction/emdash>

> **emdash** is a cross-platform UI layer for running multiple coding agents in parallel — currently supporting **OpenAI Codex CLI, Claude Code CLI, Droid (Factory CLI), Gemini CLI, Cursor CLI, Amp Code CLI, GitHub Copilot CLI, and Charm CLI**. Each agent runs in its own Git worktree so you can fan out tasks, keep changes compartmentalized, and manage everything from a single UI.

### Adjacent tools

- Conductor (Claude Code worktrees): <https://conductor.build/>
- Factory (Droid CLI): <https://factory.ai/>
- Charm: <https://charm.land/>
