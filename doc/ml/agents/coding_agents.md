---
title: Coding agents — landscape (Cursor / Cline / Aider / Windsurf / Codex / Claude Code / OpenCode / DeepCode / Amp)
main_link: https://www.swebench.com/
keywords: [coding-agents, cursor, aider, cline, codex, claude-code, opencode, deepcode, amp, swe-bench]
status: reviewed
---

# Coding agents — landscape

**Main link:** <https://www.swebench.com/>

## Summary

The 2024-2025 explosion of AI **coding agents** has produced a confusing menagerie. They differ on three orthogonal axes: **form factor** (IDE plugin vs CLI vs SaaS workspace vs autonomous bot), **autonomy** (autocomplete → edit-suggest → multi-file edits → fully-autonomous PR), and **openness** (closed proprietary vs open source). This article maps the landscape, picks reference points (Cursor / Aider / Claude Code / Codex CLI / OpenHands as the five poles), and grounds the comparison in the SWE-Bench leaderboard reality. Sibling articles cover specific tools: [[devin]], [[devon]], [[emdash]], [[smolagents]], plus `[[../llm/runtimes/goose|goose]]`.

## Insight

The picker question is **how much of the loop you want to keep**:

| Form factor | Examples | You stay in… | Best for |
|---|---|---|---|
| IDE-integrated autocomplete | Copilot, Cursor Tab, Codeium, JetBrains AI | The editor, every keystroke | Day-to-day typing acceleration |
| IDE chat-and-edit | Cursor (Cmd-K, Composer), Continue.dev, Cody | The editor, per-task | Multi-file refactors with review |
| Terminal CLI agent | Aider, Claude Code, Codex CLI, Goose, OpenCode | The terminal | Repo-aware tasks, scripting, CI |
| Background autonomous | Devin, OpenHands, Copilot Workspace | A web UI; the agent runs alone | Issue → PR pipelines |
| Multi-agent fan-out UI | emdash | A wrapper UI over multiple CLIs | Parallel exploration |

**Pricing reality (late 2025):** the closed frontier-model agents (Cursor Pro, Claude Code, Codex, Devin) cluster around $20-500/month per developer; the open ones (Aider, Cline, Goose, Continue.dev) are free but you BYO API keys and usually still pay the model provider. Aider and Cline are the most popular OSS options; Cursor and Claude Code are the most popular closed options. Windsurf (Codeium's IDE) was acquired by Google's DeepMind/Anthropic in mid-2025 — its trajectory is uncertain.

**SWE-Bench leaderboard reality.** SWE-Bench (resolve real GitHub issues end-to-end) is the standard benchmark. SWE-Bench Verified (a hand-cleaned subset) is the version everyone quotes. By late 2025 the top-of-leaderboard scores are 70-80% on Verified, up from Devin's headline-grabbing 13.86% in March 2024. The benchmark is increasingly saturated and contaminated; the next reference is **SWE-Bench Multilingual** and **Multi-SWE-Bench** for non-Python language coverage.

**The under-appreciated axis: human-in-the-loop frequency.** The autonomous-PR pitch sells well but the steady-state-cost reality is: agents that ask too much waste your attention; agents that ask too little waste your money fixing their mistakes. The Anthropic *Building Effective Agents* article ([[claude]]) frames this as the "evaluator-optimizer" question — explicit checkpoints earn their compute back.

**Vs the no-tools baseline.** An honest comparison: most "agent" wins on production codebases come from giving the model **good tools** (file search, language servers, test runners), not from agentic looping per se. A well-prompted Claude in chat with read/edit tools beats a poorly-tooled agentic loop most days.

## Similar / related topics

- **Cursor** — closed IDE fork of VS Code; the de facto IDE-with-AI by mindshare.
- **Aider** — terminal CLI; pioneered the "git-aware coding agent" pattern (every edit is a commit).
- **Claude Code** — Anthropic's official CLI; ships with `CLAUDE.md` per-repo conventions.
- **Codex CLI / OpenAI Codex** — OpenAI's official CLI; introduced the `AGENTS.md` convention.
- **OpenHands (was OpenDevin)** — open-source autonomous bot; see [[devin]].
- **Goose** — Block's open vendor-neutral CLI; see `[[../llm/runtimes/goose|goose]]`.
- **GitHub Copilot Workspace** — GitHub's autonomous-PR experiment.
- **Cline / Roo Code** — popular open VS Code extension agents.
- **Continue.dev** — open IDE/CLI extension, BYO model.
- **Windsurf (Codeium)** — IDE with deep agentic features; acquisition turbulence in 2025.
- **Amp Code** — Sourcegraph's coding agent (paid + free tier with ads).
- **OpenCode (sst/opencode)** — open CLI agent with build/plan mode separation.
- **DeepCode (HKUDS/DeepCode)** — Paper2Code / Text2Frontend / Text2Backend pipelines.

## Internal links

<!-- reviewed -->
- [[README]] — agents section landing.
- [[devin]] — Devin (Cognition) + OpenHands consolidated article.
- [[devon]] — Phorm/Devon (separate project).
- [[emdash]] — UI for running multiple coding-agent CLIs in parallel.
- [[claude]] — Anthropic's *Building effective agents* (the design framing).
- [[agent_instructs]] — `.cursorrules` / `AGENTS.md` / `CLAUDE.md` per-repo conventions.
- [[smolagents]] — minimal Python agent library (more general-purpose, can be code-focused).
- [[../llm/runtimes/goose|goose]] (P5.AD) — Block's open agent CLI.
- [[../llm/models/jetbrains_mellum|jetbrains_mellum]] (P5.AD) — example of a coding-specific FIM model.

## Keywords

`#coding-agents` `#cursor` `#aider` `#cline` `#codex` `#claude-code` `#swe-bench`

## References / raw notes

### Reference points (alphabetical, with one-line picker)

- **Aider** — <https://aider.chat/> — terminal CLI, git-aware (every edit becomes a commit).
- **Amp Code** — <https://ampcode.com/> — Sourcegraph's coding agent. Free tier (ad-supported) and paid `smart` tier per-thread. Uses a mix of OSS, production and pre-release frontier models.
- **Cursor** — <https://cursor.com/> — closed IDE fork of VS Code; Composer (multi-file), Cmd-K (inline), Tab (autocomplete).
- **Claude Code** — <https://claude.com/product/claude-code> — Anthropic CLI; `CLAUDE.md` convention.
- **Cline** — <https://github.com/cline/cline> — open VS Code extension agent (formerly Claude Dev).
- **Codex CLI** — <https://github.com/openai/codex> — OpenAI's official CLI; `AGENTS.md` convention.
- **Continue.dev** — <https://www.continue.dev/> — open IDE extension + CLI.
- **Cody** — <https://sourcegraph.com/cody> — Sourcegraph's IDE chat (older sibling of Amp).
- **DeepCode** — <https://github.com/HKUDS/DeepCode> — Paper2Code / Text2Frontend / Text2Backend pipelines.
- **emdash** — see [[emdash]].
- **GitHub Copilot Workspace** — <https://github.blog/news-insights/product-news/github-copilot-workspace/>.
- **Goose** — see `[[../llm/runtimes/goose|goose]]`.
- **OpenCode** — <https://github.com/sst/opencode> — open CLI with two modes:
  - **build** — default, full file-edit & shell access.
  - **plan** — read-only analysis; denies edits, asks before bash.
  - Plus a built-in `@general` subagent for multi-step tasks.
- **OpenHands (was OpenDevin)** — see [[devin]].
- **Phind**, **Replit Agent**, **v0** — adjacent products with narrower scopes.
- **Roo Code** — Cline fork with broader provider support.
- **Windsurf** — <https://windsurf.com/> — Codeium's IDE; trajectory uncertain post-acquisitions.

### Benchmarks

- **SWE-Bench** — <https://www.swebench.com/> — original Princeton benchmark (Jimenez et al. 2023); 2,294 real GitHub issues from 12 popular Python repos.
- **SWE-Bench Verified** — OpenAI-curated 500-issue subset (Aug 2024) that's the most-quoted leaderboard today.
- **SWE-Bench Multilingual / Multi-SWE-Bench** — non-Python coverage.
- **CodeRAGBench**, **LiveCodeBench**, **HumanEval+/MBPP+** — narrower scopes (retrieval, contest-style, function-level).

### Tags

`#agent` `#coding` `#free` `#open` `#AI`
