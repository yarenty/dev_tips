---
title: "Workmux — git worktrees + tmux windows for parallel dev (and AI agents)"
main_link: https://github.com/raine/workmux
keywords: [workmux, tmux, git-worktree, parallel-dev, ai-agents, tmuxinator-alternative]
status: reviewed
review_date: 2026/05/03
---

# Workmux — git worktrees + tmux windows

**Main link:** <https://github.com/raine/workmux>

## Summary

Workmux is a "giga-opinionated, zero-friction" CLI that orchestrates **git worktrees** and **tmux windows** as paired, isolated development environments. One worktree → one tmux window with a pre-defined pane layout, post-creation hooks (deps install, DB setup), and copied/symlinked config files (`.env`, `node_modules`). When you're done: one `workmux merge` cleans up the branch, the worktree, and the tmux window in one shot. Author: Raine Virta. Aimed squarely at workflows where you want to **run multiple branches — or multiple AI agents — in parallel without conflict**.

## Insight

The premise is sharp: **tmux is the interface**. If you already live in tmux, you don't need yet another TUI to manage your work — you need a tool that turns "make a branch, set up an editor + a watch + a shell, get going" into one command, and the inverse into another.

Reach for it when:

- You routinely have **3–5 branches in flight** and you're tired of `git stash` ping-pong.
- You're driving **multiple AI coding agents** (Claude Code, Aider, Cursor agents) in parallel — each gets its own worktree, its own tmux window, its own pane layout, no cross-contamination.
- Your project's bootstrap (install, build, dev-server, watcher) is non-trivial and you want it codified in a `.workmux.yaml` rather than a tribal-knowledge `README.md`.

It's an opinionated take on the same space as Tmuxinator / Smug / Sesh, but built around git worktrees as the primary unit. If you've never used `git worktree`, the README's _"Why git worktrees?"_ section is worth reading first.

Comparisons:

- vs **Tmuxinator** — Tmuxinator is window/session layout only; Workmux adds the worktree lifecycle.
- vs **Smug** — same idea as Tmuxinator, in Go; same gap on worktrees.
- vs **Sesh** — fuzzy session switcher; complementary, not overlapping.
- vs **plain `git worktree`** — Workmux is the ergonomic shell on top.

Caveats: tmux-only by design (so no joy if you live in [[zellij]]); strong opinions about pane layout (you'll be editing `.workmux.yaml` early); the "AI agents" pitch is real but you still need to wire up the agents themselves.

## Similar / related topics

- [Tmuxinator](https://github.com/tmuxinator/tmuxinator) — Ruby, tmux session layouts.
- [Smug](https://github.com/ivaaaan/smug) — Go reimplementation of Tmuxinator.
- [Sesh](https://github.com/joshmedeski/sesh) — fuzzy tmux session switcher.
- [`git worktree`](https://git-scm.com/docs/git-worktree) — the underlying primitive.
- [[zellij]] — different multiplexer if you've left tmux behind.

## Internal links

- [[tools/linux/tmux|tmux]] — the substrate Workmux drives.
- [[bacon]] — drop a bacon pane into your worktree's tmux layout for free Rust feedback.
- [[prek]] — keep each parallel worktree lint-clean at commit time.
- [[lazygit]] — Git side; pair with Workmux for full workflow.

## Keywords

`#workmux` `#tmux` `#git-worktree` `#parallel-dev` `#ai-agents` `#tmuxinator-alternative` `#misc` `#tools`

## References / raw notes

> **Git worktrees + tmux windows**
>
> Giga opinionated zero-friction workflow tool for managing [git worktrees](https://git-scm.com/docs/git-worktree) and tmux windows as isolated development environments. Perfect for running multiple AI agents in parallel without conflict.

### Philosophy

- **One worktree, one tmux window**: each git worktree gets its own dedicated, pre-configured tmux window.
- **Frictionless**: multi-step workflows are reduced to simple commands.
- **Configuration as code**: define your tmux layout and setup steps in `.workmux.yaml`.

The core principle is that **tmux is the interface**. If you already live in tmux, you shouldn't need to learn a new TUI app or separate interface to manage your work. With Workmux, managing parallel development tasks — or multiple AI agents — is as simple as managing tmux windows.

### Features

- Create git worktrees with matching tmux windows in a single command (`add`).
- Automatically set up your preferred pane layout (editor, shell, watchers, etc.).
- Run post-creation hooks (install dependencies, set up database, etc.).
- Copy or symlink configuration files (`.env`, `node_modules`) into new worktrees.
- Merge branches and clean up everything (worktree, tmux window, branches) in one command (`merge`).
- List all worktrees with their tmux and merge status.
- Bootstrap projects with an initial configuration file (`init`).
- Dynamic shell completions for branch names.

Demo: <https://raw.githubusercontent.com/raine/workmux/refs/heads/main/meta/workmux-demo.gif>

Repo: <https://github.com/raine/workmux>
