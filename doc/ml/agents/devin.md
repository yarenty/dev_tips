---
title: Devin (Cognition AI) & OpenDevin / OpenHands — autonomous coding agents
main_link: https://www.cognition-labs.com/introducing-devin
keywords: [devin, cognition, opendevin, openhands, swe-bench, autonomous-coding-agent]
status: reviewed
---

# Devin (Cognition AI) & OpenDevin / OpenHands

**Main link:** <https://www.cognition-labs.com/introducing-devin>

## Summary

**Devin** (Cognition AI, March 2024 launch) is the closed-source "autonomous AI software engineer" whose [13.86% SWE-Bench score](https://www.cognition-labs.com/introducing-devin) — vs the prior 1.96% state-of-the-art — kicked off the modern coding-agent gold rush. Within weeks the open-source community responded with **OpenDevin** (later rebranded **OpenHands** after a trademark concern in 2024), aiming to replicate Devin in the open. Both products give you the same UX: an agent that takes a natural-language task, plans, edits files in a sandboxed VM, runs tests, debugs, and submits a PR — humans optional. This article covers both as a pair because the comparison is the interesting part.

## Insight

The honest framing two years on is:

- **Devin's headline claim was real, but the field caught up fast.** 13.86% on SWE-Bench was genuinely a leap in March 2024 (the previous best, given the exact files to edit, was 4.80%). By late 2025 multiple agents — including OpenHands, [SWE-agent](https://swe-agent.com/), and various Claude-Code-with-tools setups — clear 60-80% on SWE-Bench Verified. **The benchmark moved faster than Devin did.**
- **Cognition pivoted.** After the initial buzz, Cognition broadened from "Devin the standalone product" to **Devin + Cognition Codex + integrations** (GitLab, Jira). Devin remains pricey ($20/seat-month entry, much higher in practice) and gated; the company's strategic value has shifted toward the *infrastructure for autonomous agents*, not the chatbot interface.
- **OpenHands won the open-clone race.** OpenHands (formerly OpenDevin) is now the canonical open-source autonomous coding agent: model-agnostic (Claude, GPT, DeepSeek, local models), runs in a Docker sandbox, has a polished web UI, hits respectable SWE-Bench scores, and is the substrate behind several commercial agents. The All-Hands.dev team maintains it.
- **What "autonomous" actually buys.** For well-scoped, test-covered bug fixes on small repos, autonomous mode genuinely works and saves the human's attention. For greenfield design work, large-codebase architectural changes, or anything underspecified, autonomous agents still consume more compute *and* more human cleanup time than a developer with [[../llm/runtimes/goose|Goose]] / Claude Code / Cursor. Pick autonomy by *task variance*, not by hype.
- **Vs Claude Code / Codex CLI / Goose.** Those are CLIs that the human drives — agentic in the loop, not autonomous. Devin/OpenHands run the loop themselves. The boundary blurs (Claude Code can take long autonomous chunks), but the operator/operand distinction matters when you're choosing an interaction style.

When to reach for which:

| Situation | Pick |
|---|---|
| You want an autonomous bot to grind through a backlog of small bugs | OpenHands (open) or Devin (closed) |
| You want autonomy but on your own infra | OpenHands |
| You want to drive the loop yourself, terminal-side | Claude Code, Codex CLI, Aider, Goose |
| You want autonomy on GitHub-issue → PR specifically | Copilot Workspace, Devin's Linear/Slack integration |
| You're benchmarking SWE-Bench-style autonomy | OpenHands or Cognition's Devin demo |

## Similar / related topics

- **Cognition AI** — the company behind Devin; Y-Combinator-funded, founded by IOI/competitive-programming alumni.
- **OpenHands** (formerly OpenDevin) — open-source clone, now the de facto open autonomous coding agent.
- **SWE-agent** — Princeton's research-grade autonomous SWE-Bench solver; influential paper.
- **Devon** — *separate* project (entropy-research/Devon); Python-only, see [[devon]].
- **GitHub Copilot Workspace** — GitHub's autonomous-PR experiment.
- **Lindy / Cognosys / MultiOn** — adjacent autonomous-agent products outside coding.
- **Cursor / Claude Code / Codex CLI** — the developer-driven side; see [[coding_agents]].

## Internal links

<!-- reviewed -->
- [[README]] — agents section landing.
- [[coding_agents]] — full coding-agent landscape (Cursor / Aider / Cline / etc.).
- [[devon]] — Devon (Phorm/entropy-research, separate project).
- [[emdash]] — UI for running multiple coding-agent CLIs in parallel.
- [[claude]] — Anthropic's *Building effective agents* (the design framing for autonomy).
- [[agent_instructs]] — `.cursorrules` / `AGENTS.md` / `CLAUDE.md` per-repo conventions.
- [[../llm/runtimes/goose|goose]] (P5.AD) — Block's open agent CLI (developer-driven counterpart).

## Keywords

`#devin` `#cognition` `#opendevin` `#openhands` `#swe-bench` `#autonomous-coding-agent`

## References / raw notes

### Devin (Cognition AI)

- Launch announcement (Mar 2024): <https://www.cognition-labs.com/introducing-devin>
- Cognition AI: <https://cognition.ai/>
- Cognition's later product line (Devin Workspaces, Devin for Slack/Linear/GitHub).
- SWE-Bench claim (Mar 2024): 13.86% end-to-end resolution on SWE-Bench, vs prior SOTA 1.96% end-to-end / 4.80% with files-given.
- Independent reproductions of the Devin demo in 2024 raised concerns about how representative the live demo was (multiple critique threads on X / Twitter); the underlying SWE-Bench numbers themselves were verifiable.

### OpenHands (was OpenDevin)

- Repo: <https://github.com/All-Hands-AI/OpenHands>
- Original OpenDevin announcement (Mar 2024): community-led replication of Devin.
- 2024 rename to **OpenHands** under the All-Hands-AI org for trademark / focus reasons; the README spells it out.
- Paper: <https://arxiv.org/abs/2407.16741> — *OpenDevin: An Open Platform for AI Software Developers as Generalist Agents.*
- Architecture: model-agnostic (LiteLLM-backed), runs each session in a Docker sandbox with a structured action space (file edit, browser, shell, IPython); web UI + headless modes.
- Frequently appears at or near the top of SWE-Bench Verified leaderboards.

### Benchmarks

- SWE-Bench: <https://www.swebench.com/>
- SWE-Bench Verified (OpenAI Aug 2024 curated subset): the headline leaderboard most agents quote.
- *SWE-bench: Can Language Models Resolve Real-World GitHub Issues?* (Jimenez et al. 2023) <https://arxiv.org/abs/2310.06770>

### Original raw notes

> *Devin* (Cognition Labs): we evaluated Devin on SWE-bench, a challenging benchmark that asks agents to resolve real-world GitHub issues found in open source projects like Django and scikit-learn. Devin correctly resolves 13.86% of the issues end-to-end, far exceeding the previous state-of-the-art of 1.96%. Even when given the exact files to edit, the best previous models can only resolve 4.80% of issues.

> *OpenDevin*: an open-source project aiming to replicate Devin, an autonomous AI software engineer who is capable of executing complex engineering tasks and collaborating actively with users on software development projects. This project aspires to replicate, enhance, and innovate upon Devin through the power of the open-source community.
