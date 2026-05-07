---
title: tools — historical "coreutils" parent (post-split index)
main_link: 
keywords: [tools, rust, cli, coreutils, index]
status: reviewed
---

# tools — historical "coreutils" parent (post-split index)

## Summary

This file is the historical parent of what is now most of the `tooling/` section. The auto-splitter previously extracted 18 sibling articles out of a single multi-topic file titled "coreutils"; what remains here is the empty husk plus a pointer at each child. For the curated thematic index of this section, see [[README]].

## Insight

The original raw notes were a single line — `cargo install coreutils` — referring to the [`uutils/coreutils`](https://github.com/uutils/coreutils) project (a Rust-language re-implementation of GNU coreutils, the `cat`/`ls`/`cp`/`mv`/`mkdir`/`rm`/etc. set). That project is genuinely interesting (Ubuntu announced in early 2025 that its 25.10 release would adopt `uutils/coreutils` by default), but it isn't the same as any of the *individual* Rust replacements covered as siblings here:

- `uutils/coreutils` aims to be a **drop-in `cat`/`ls`/`cp`** that matches GNU semantics byte-for-byte — invisible by design.
- The siblings ([[bat]], [[exa]], [[dust]], [[ripgrep]], [[bottom]], etc.) are deliberately ***not* drop-ins** — they choose new defaults, new flags, new output formats.

You almost never want both: `uutils` if you want the GNU semantic invariants but a Rust implementation; the siblings if you want a friendlier UX.

For the actual section index — which tool to reach for in which situation — see [[README]].

## Similar / related topics

- [`uutils/coreutils`](https://github.com/uutils/coreutils) — the actual "Rust coreutils" project the original note pointed at.
- [[bat]] / [[exa]] / [[dust]] / [[ripgrep]] / [[bottom]] / [[bandwhich]] / [[zoxide]] / [[topgrade]] / [[difftastic]] / [[lychee]] — the friendlier-than-GNU siblings.

## Internal links

<!-- reviewed -->

- [[README]] — the curated thematic index for this section.
- [[bat]] / [[exa]] / [[dust]] / [[ripgrep]] / [[bottom]] — Unix-replacement siblings.
- [[zellij]] / [[mprocs]] — multiplexers.
- [[starship]] / [[zoxide]] / [[watchexec]] — shell productivity.

## Keywords

`#tools` `#rust` `#cli` `#coreutils` `#uutils` `#index`

## References / raw notes

- Original cryptic note: `cargo install coreutils` — referring to [`uutils/coreutils`](https://github.com/uutils/coreutils) (Rust re-implementation of GNU coreutils; Ubuntu 25.10+ default).

This file was the auto-split parent of these siblings:

- [[dust]] (was `dusk_replacement_of_du`) — `du` replacement.
- [[mprocs]] — multi-process TUI orchestrator.
- [[zellij]] — terminal multiplexer.
- [[ripgrep]] — `grep` replacement.
- [[bat]] — `cat` replacement.
- [[exa]] — `ls` replacement.
- [[bottom]] — `top` replacement.
- [[rust_in_jupyter]] — `evcxr_jupyter` kernel.
- [[zoxide]] — smarter `cd`.
- [[clido]] — CLI to-do list.
- [[joshuto]] — terminal file manager.
- [[topgrade]] — package-manager orchestrator.
- [[bandwhich]] — bandwidth-by-process top.
- [[starship]] — cross-shell prompt.
- [[difftastic]] — syntax-aware diff.
- [[armada]] — TCP SYN scanner.
- [[watchexec]] — file-watch runner.
- [[lychee]] — link checker.
