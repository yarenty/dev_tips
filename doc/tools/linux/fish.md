---
title: Fish
main_link: https://github.com/starship/starship
keywords: [fish, linux, julia, evans, reasons, tmux, shell]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Fish

**Main link:** <https://github.com/starship/starship>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tools/linux/tmux|tmux]] — TMUX _(score 24.3)_
- [[tmux_ai]] — Tmux AI _(score 24.3)_
- [[starshio]] — Starshio _(score 24.3)_
- [[tools/linux/zellij|zellij]] — Zellij _(score 24.3)_
- [[tools/shell/tmux|tmux]] — Tmux _(score 21.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#fish` `#linux` `#tools` `#julia` `#evans` `#reasons` `#shell`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 1 additional top-level heading(s) extracted into sibling files:
> - [Starshio](starshio.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Fish
Julia Evans recently posted Reasons I still love the fish shell, and the first point she makes is “no configuration”.

Things that require plugins and lots of code in shells like ZSH, like autosuggestions, are included and configured by default in fish. At the time of writing this, my fish config has less than 31 loc, most of which are abbreviations.

I have two fish plugins configured: z for jumping to a directory, and hydro as my shell prompt. Neither need configuration.

## Installing
