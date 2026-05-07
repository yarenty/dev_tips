---
title: mprocs — multi-process TUI orchestrator
main_link: https://github.com/pvolok/mprocs
keywords: [mprocs, rust, procfile, foreman, overmind, hivemind, tui, dev]
status: reviewed
---

# mprocs — multi-process TUI orchestrator

**Main link:** <https://github.com/pvolok/mprocs>

## Summary

`mprocs` is a Rust TUI by Pavel Volok that runs *N* commands in parallel, each in its own pane, with one combined keymap to switch focus / send keys / restart. The classic use is the local-dev workflow where you'd otherwise have a tmux window per service: `web`, `worker`, `db`, `redis`, `tail -f log`. Configuration is a single `mprocs.yaml` next to your repo describing the processes to start.

## Insight

Reach for `mprocs` when:

- You have a known fixed set of long-running dev processes that you always start together.
- You want to *see all logs at once* in one window, switch focus to one with a key, and not deal with `tmux` ergonomics.
- You want the config in version-controlled YAML so a teammate can `mprocs` and reproduce your dev session.

Sample config:

```yaml
# mprocs.yaml
procs:
  web:
    cmd: ['cargo', 'run', '--bin', 'web']
  worker:
    cmd: ['cargo', 'run', '--bin', 'worker']
  db:
    shell: 'docker compose up postgres'
  redis:
    shell: 'docker compose up redis'
  logs:
    shell: 'tail -F logs/*.log'
```

**Compared to siblings**:

- **Procfile + [`foreman`](https://github.com/ddollar/foreman) (Ruby)** / **`forego` (Go)** / **`honcho` (Python)** — same idea, no TUI; logs interleaved into stdout. Good for `heroku local`-style "just run it".
- **[`overmind`](https://github.com/DarthSim/overmind) (Go)** — Procfile + tmux, attach to a single process for interactive use. The most direct competitor to `mprocs`.
- **[`hivemind`](https://github.com/DarthSim/hivemind)** — overmind's lighter sibling without tmux.
- **`docker compose up`** — equivalent if every service is already a container.
- **[[zellij]] / `tmux`** — general-purpose multiplexers; `mprocs` is the *configured workspace* shape.

Pick `mprocs` when (a) you want a TUI and (b) you don't already have tmux muscle memory. Pick `overmind` when you have tmux muscle memory and want Procfile compatibility. Pick `docker compose` when everything is containerised already.

**Gotchas**: `mprocs` doesn't auto-restart on file change (pair with [[watchexec]] or `cargo watch` inside the command); shell-vs-cmd distinction matters for shell expansion; the YAML format has changed across versions — pin a version in CI.

## Similar / related topics

- [`foreman`](https://github.com/ddollar/foreman) (Ruby) / `forego` (Go) / `honcho` (Python) — Procfile runners.
- [`overmind`](https://github.com/DarthSim/overmind) — Procfile + tmux; closest competitor.
- `docker compose up` — container-shaped equivalent.
- [[zellij]] / `tmux` — general-purpose multiplexers.
- [[watchexec]] — pair with mprocs to auto-restart processes on file change.

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[zellij]] — sibling but general-purpose multiplexer.
- [[../../../tools/linux/tmux|tools/linux/tmux]] — tmux article (overmind's substrate).
- [[watchexec]] — file-watch runner; pair with mprocs.

## Keywords

`#mprocs` `#rust` `#tui` `#procfile` `#foreman` `#overmind` `#dev-workflow` `#orchestration`

## References / raw notes

- Repo: <https://github.com/pvolok/mprocs>
- Crate: <https://crates.io/crates/mprocs>
- Install: `cargo install mprocs` or `brew install mprocs`.
- Use case: long-running dev processes (DB, server, log tail) side-by-side in one TUI.
