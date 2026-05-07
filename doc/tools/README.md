---
title: Tools
keywords: [tools, cli, terminal, shell, linux, macos, design, editors, networking, security]
status: reviewed
review_date: 2026/05/03
---

# Tools

A grab-bag of CLI / terminal / desktop tools that don't fit cleanly into any other section. Organised by what kind of tool it is, not by language.

## Subsections

| Section | What's in it |
|---------|---------------|
| [[tools/linux/README\|linux]] | Terminal multiplexers (tmux, zellij, byobu), shells (fish), editors (helix), git UIs (lazygit), networking (mosh, trace), debuggers, reading list. |
| [[tools/shell/README\|shell]] | POSIX shell utilities — text-mode browsers (carbonyl, browsh, lynx), shell-script ergonomics (must_have, tools, color_codes), SSH helpers (sshpass), DNS tricks (dns_toys). |
| [[tools/osx/README\|osx]] | macOS-specific tooling — iOS dev, Gatekeeper / quarantine tricks, OSXCross cross-compile, UTM virtualization. |
| [[tools/editors/README\|editors]] | Editor / IDE-specific notes (Zed, Cursor, Void, etc.). |
| [[tools/networking/README\|networking]] | Network analysis / tunneling tools. |
| [[tools/security/README\|security]] | Security / forensics / WireGuard tooling. |
| [[tools/design/README\|design]] | Design / wireframing / icons. |
| [[tools/live_demos/README\|live_demos]] | Recording terminal demos (vhs, asciinema, terminalizer). |
| [[tools/misc/README\|misc]] | Genuinely misc — calc, finance trackers, payments, wifi tools, Windows CLI niceties, etc. |

## Decision shortcuts

- "I want a friendlier shell" → [[fish]]
- "I want my git workflow to stop hurting" → [[lazygit]]
- "I want my SSH session to survive Wi-Fi roaming" → [[mosh]] + [[tools/linux/tmux|tmux]] (or [[zellij]])
- "I want to build a macOS binary on Linux CI" → [[osxcross]]
- "I want to record a terminal demo for a README" → see [[tools/live_demos/README|live_demos]]
- "I need a one-line install of useful CLI tools on a fresh box" → [[must_have]]

## Cross-section see-also

- [[diagrams]] — diagrams.net + D2 for architecture diagrams.
- [[manim]] — programmatic animations for math / explainer content.
- [[fonts]] — programming-font picks.
- [[oracle_free_tier]] — natural target host for self-installed tools.

## Keywords

`#tools` `#cli` `#terminal` `#shell` `#linux` `#macos` `#design` `#editors` `#networking` `#security`
