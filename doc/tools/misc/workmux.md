
https://github.com/raine/workmux



<p align="center">
  <strong>Git worktrees + tmux windows</strong>
</p>

---

Giga opinionated zero-friction workflow tool for managing
[git worktrees](https://git-scm.com/docs/git-worktree) and tmux windows as
isolated development environments. Perfect for running multiple AI agents in
parallel without conflict.

→ [Why git worktrees?](#why-git-worktrees)

![workmux demo](https://raw.githubusercontent.com/raine/workmux/refs/heads/main/meta/workmux-demo.gif)

## Philosophy

- **One worktree, one tmux window**: Each git worktree gets its own dedicated,
  pre-configured tmux window.
- **Frictionless**: Multi-step workflows are reduced to simple commands.
- **Configuration as code**: Define your tmux layout and setup steps in
  `.workmux.yaml`.

The core principle is that **tmux is the interface**. If you already live in
tmux, you shouldn't need to learn a new TUI app or separate interface to manage
your work. With workmux, managing parallel development tasks, or multiple AI
agents, is as simple as managing tmux windows.

## Features

- Create git worktrees with matching tmux windows in a single command (`add`)
- Automatically set up your preferred pane layout (editor, shell, watchers,
  etc.)
- Run post-creation hooks (install dependencies, setup database, etc.)
- Copy or symlink configuration files (`.env`, `node_modules`) into new
  worktrees
- Merge branches and clean up everything (worktree, tmux window, branches) in
  one command (`merge`)
- List all worktrees with their tmux and merge status
- Bootstrap projects with an initial configuration file (`init`)
- Dynamic shell completions for branch names