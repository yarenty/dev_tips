---
title: Termion
main_link: https://gitlab.redox-os.org/redox-os/termion
keywords: [termion, rust, redox, tty, api, linux]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/tui.md` by `article_split.py`. Heading: **Termion**.

# Termion

**Main link:** <https://gitlab.redox-os.org/redox-os/termion>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tui]] — Ratatui _(score 27.6)_
- [[iocraft]] — IOCraft _(score 23.9)_
- [[paho_mqtt]] — paho MQTT _(score 19.9)_
- [[indicatif]] — Indicatif _(score 18.7)_
- [[rtic]] — RTIC _(score 14.7)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#termion` `#rust` `#programming` `#redox` `#tty` `#api` `#library`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Termion

https://gitlab.redox-os.org/redox-os/termion


Termion is a pure Rust, bindless library for low-level handling, manipulating
and reading information about terminals. This provides a full-featured
alternative to Termbox.
Termion aims to be simple and yet expressive. It is bindless, meaning that it
is not a front-end to some other library (e.g., ncurses or termbox), but a
standalone library directly talking to the TTY.
Termion is quite convenient, due to its complete coverage of essential TTY
features, providing one consistent API. Termion is rather low-level containing
only abstraction aligned with what actually happens behind the scenes. For
something more high-level, refer to inquirer-rs, which uses Termion as backend.
Termion generates escapes and API calls for the user. This makes it a whole lot
cleaner to use escapes.
Supports Redox, Mac OS X, BSD, and Linux (or, in general, ANSI terminals).
