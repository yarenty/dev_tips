---
title: Devon — entropy-research's open coding agent (not Devin!)
main_link: https://github.com/entropy-research/Devon
keywords: [devon, entropy-research, coding-agent, python, open-source]
status: reviewed
review_date: 2026/05/03
---

# Devon — entropy-research's open coding agent

**Main link:** <https://github.com/entropy-research/Devon>

## Summary

**Devon** (entropy-research/Devon) is an open-source coding agent — *not* to be confused with **[[devin|Devin]]** (Cognition AI). Same agentic-coding territory, similar UX (multi-file edits, codebase exploration, test writing, bug fixing), but a different team, much smaller scope, and explicitly Python-focused. The repo went quiet through 2024-2025 and the project has lost ground to better-resourced alternatives like [[../llm/runtimes/goose|Goose]], OpenHands, Cline, and Aider; it's filed here mostly for historical completeness and to disambiguate the name collision.

## Insight

The honest story:

- **Naming.** *Devin* (Cognition) and *Devon* (entropy-research) launched within weeks of each other in early 2024. Both wrote agentic Python coding agents. The names confused everyone. This article exists to keep the two cleanly separated in the vault graph.
- **Scope.** Devon's feature list (multi-file editing, codebase exploration, config/test/bug-fixing) is the *table-stakes* coding-agent feature set. By 2025 these are commodity capabilities in every CLI agent.
- **Python-only.** This is the practical limitation that pushed users to alternatives: by mid-2024 Cursor, Aider, Claude Code, and OpenHands all worked across languages.
- **Maintenance.** The repo's recent activity is sparse; the original `tldrai`-source notes flagged "not working — no gpt4-o available" which dates to the GPT-4o rollout (May 2024). Treat as **archive-tier**.
- **When to look at it anyway.** If you're studying agent-architecture history (the early-2024 wave), Devon was a clean small-codebase reference implementation. For *production* agentic coding, pick from [[coding_agents]].

## Similar / related topics

- **Devin (Cognition AI)** — the closed, much-better-funded namesake; see [[devin]].
- **OpenHands (was OpenDevin)** — open-source autonomous coding agent that took the OSS niche; see [[devin]].
- **Aider** — terminal-CLI coding agent, git-aware, much more momentum.
- **Cline / Roo Code** — open VS Code extension agents.
- **Goose** — Block's open vendor-neutral CLI; see `[[../llm/runtimes/goose|goose]]`.

## Internal links

- [[README]] — agents section landing.
- [[devin]] — Devin (Cognition AI) + OpenHands; the namesake to keep distinct.
- [[coding_agents]] — coding-agent landscape (where to look instead).
- [[../llm/runtimes/goose|goose]] (P5.AD) — open agent CLI to consider as a replacement.
- [[../programming/rust/ml/ml_in_rust|ml_in_rust]] (P5.Y) — for the Rust-side ML library landscape (the auto-suggested `[[rust_ml]]` reference is now stale; this is the canonical replacement).

## Keywords

`#devon` `#entropy-research` `#coding-agent` `#python` `#open-source`

## References / raw notes

- Repo: <https://github.com/entropy-research/Devon>
- entropy-research org: <https://github.com/entropy-research>

### Features (per the original README)

- Multi-file editing
- Codebase exploration
- Config writing
- Test writing
- Bug fixing
- Architecture exploration

### Limitations

- Minimal functionality for non-Python languages.
- Sometimes the user has to specify the file where the change should happen.
- Original note flagged "not working — no gpt4-o available" (timing puts this at the May 2024 GPT-4o rollout).
