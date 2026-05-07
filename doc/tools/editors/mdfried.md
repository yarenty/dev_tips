---
title: "mdfried — terminal Markdown viewer with bigger headings"
main_link: https://github.com/benjajaja/mdfried
keywords: [mdfried, markdown, terminal, tui, ratatui, rust, viewer, cli]
status: reviewed
---

# mdfried — terminal Markdown viewer with bigger headings

**Main link:** <https://github.com/benjajaja/mdfried>

## Summary

`mdfried` is a small Rust TUI Markdown viewer that renders headers as **Bigger Text** (block-letter style) so the visual hierarchy of a document actually pops in a terminal pane. It's built on Ratatui and shipped as a single binary — `brew install mdfried` or `cargo install mdfried`.

## Insight

Most terminal renderers (`glow`, `mdcat`, `bat -l md`) make headers slightly bolder and call it a day. `mdfried` actually scales the heading text up using block-glyph fonts, which makes it surprisingly useful for:

- Reading long READMEs without leaving the terminal.
- Previewing slide-deck-shaped Markdown before piping it through [[marp]].
- Working over SSH where you don't want to launch a GUI.

It's not trying to replace `glow` or your editor's preview — it's a single trick done well. If you want full-fat rendering with images and links, stick with `glow`; if you want the page structure to be readable from across the room, `mdfried` wins.

## Similar / related topics

- [glow](https://github.com/charmbracelet/glow) — Charm's polished terminal Markdown renderer.
- [mdcat](https://github.com/swsnr/mdcat) — `cat` for Markdown, supports inline images via Kitty/iTerm protocols.
- `bat -l md` — quick-and-dirty syntax-highlighted Markdown.
- [[markitdown]] — produce the Markdown that `mdfried` then displays.
- [[marp]] — turn Markdown into actual slides.

## Internal links

- [[markitdown]] — generate Markdown from PDF/DOCX/etc., then preview it here.
- [[marp]] — Markdown → slide decks.
- [[helix]] — terminal editor; pair `mdfried` for split-pane preview.
- [[tools/linux/tmux|tmux]] — keep editor + `mdfried` in adjacent panes.

## Keywords

`#mdfried` `#markdown` `#terminal` `#tui` `#ratatui` `#rust` `#cli` `#editors`

## References / raw notes

> `mdfried` is a markdown viewer for the terminal that renders headers as **Bigger Text** than the rest.

Tags from the source: `#markdown` `#tool` `#cli` `#terminal` `#rust` `#shell` `#tui` `#ratatui` `#brew`

Install (Homebrew): `brew install mdfried`

Screenshot: <https://github.com/benjajaja/mdfried/raw/master/assets/screenshot_1.png>
