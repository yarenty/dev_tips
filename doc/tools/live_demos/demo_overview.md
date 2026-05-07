---
title: "demo-magic — scripted terminal demos with fake typing"
main_link: https://github.com/paxtonhare/demo-magic
keywords: [demo-magic, live-demo, presentation, bash, terminal, kubecon, conference, scripting]
status: reviewed
---

# demo-magic — scripted terminal demos with fake typing

**Main link:** <https://github.com/paxtonhare/demo-magic>

Sample template: <https://github.com/paxtonhare/demo-magic/blob/master/samples/demo-template.sh>

## Summary

`demo-magic.sh` is a small bash library by Paxton Hare for running **scripted live demos** in a terminal. You write a shell script that calls helpers like `pe "kubectl get pods"` (print + execute), `p "comment line"` (print only), `wait` (pause for keypress), and the audience sees commands appear character-by-character as if you were typing them — with no typos and no broken pipes. Sourced once at the top of your demo script.

## Insight

If you've watched a slick KubeCon demo where the speaker "types" 30 lines of perfect YAML in 4 seconds and hits return, this is almost certainly what they're using. The use case is specific but high-value:

- **Conference talks** where you have 30 minutes and zero tolerance for a typo restarting your flow.
- **Workshop walk-throughs** where you want the audience to see commands appear naturally, but you don't trust your own fingers.
- **Recorded screencasts** where you want a "live" feel without the editing.
- **Internal demos** where the dependency stack is too fragile to actually type-and-pray.

It's bash, single file, no dependencies — just `source demo-magic.sh` at the top of your script. Pair with [[tools/linux/tmux|tmux]] panes if you need split layouts.

Comparisons:

- vs **`asciinema`** — `asciinema` _records_ a real terminal session and replays it; `demo-magic` _scripts_ a fake-typed live one. Different tools.
- vs **`vhs`** (Charm) — `vhs` produces GIF/MP4 from a `.tape` script; great for README demos. `demo-magic` is for in-person, in-shell demos.
- vs **`terminalizer`** — record-and-replay like asciinema but with a more polished output; again, not the same niche.
- vs **doing it for real** — only do it for real if your demo is bulletproof. Mine never is.

Caveats: bash-only (no Windows native), the typing speed and pauses need tuning per audience (`TYPE_SPEED`), and `set -e`/error handling is your responsibility.

```bash
#!/usr/bin/env bash
. demo-magic.sh -d -n -w 3   # no clear, no wait between commands, 3s timeout

TYPE_SPEED=20

p "# spin up a cluster"
pe "k3d cluster create demo"
pe "kubectl get nodes"
wait
```

## Similar / related topics

- [`asciinema`](https://asciinema.org/) — record real terminal sessions; share as a player.
- [`vhs`](https://github.com/charmbracelet/vhs) — Charm: scripted terminal → GIF/MP4 for READMEs.
- [`terminalizer`](https://terminalizer.com/) — record + render terminal sessions to GIF.
- [`presenterm`](https://github.com/mfontanini/presenterm) — Markdown slide deck inside the terminal (see [[term]]).
- [Reveal.js](https://revealjs.com/) — when you've decided the terminal isn't enough.

## Internal links

- [[term]] — `presenterm` for Markdown-driven terminal slide decks; pair with `demo-magic` for the live-shell sections.
- [[tools/linux/tmux|tmux]] — split panes for multi-shell demos.
- [[zellij]] — modern multiplexer alternative if `tmux` isn't your style.

## Keywords

`#demo-magic` `#live-demo` `#presentation` `#bash` `#terminal` `#kubecon` `#conference` `#scripting` `#tools`

## References / raw notes

> # DEMO
>
> how to prepare automatic presentation - CODE in live!
>
> <https://github.com/paxtonhare/demo-magic>
>
> <https://github.com/paxtonhare/demo-magic/blob/master/samples/demo-template.sh>
>
> Jaro: Just run all you need - press enter when needed - no errors in typing... and cheating behind the scene!
