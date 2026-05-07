---
title: presenterm — markdown presentations in the terminal
main_link: https://github.com/mfontanini/presenterm
keywords: [presenterm, presentations, terminal, markdown, slides, kitty, mfontanini]
status: reviewed
review_date: 2026/05/03
---

# presenterm — markdown presentations in the terminal

**Main link:** <https://github.com/mfontanini/presenterm>

## Summary

`presenterm` is a Rust CLI by Matías Fontanini for **giving slide presentations directly in the terminal** from a single Markdown file. It supports inline images and animated GIFs (via the Kitty / iTerm2 / Sixel image protocols), customisable themes, syntax-highlighted code blocks, speaker notes, slide transitions, and PDF export. You write `slides.md`, run `presenterm slides.md`, and present.

## Insight

Reach for it whenever you want a **dev-friendly, version-controllable presentation** — conference talks, brown-bags, screen-recorded demos, or anywhere a `.pptx` would feel out of place. Massive practical wins: slides live in your repo, diff cleanly, and code blocks render with proper syntax highlighting (no more pasted screenshots). Best on a Kitty / WezTerm / Ghostty terminal that supports the graphics protocol — fall back to ASCII art or PDF export elsewhere.

Caveats: live-coding inside slides is not the goal (use `asciinema` / `vhs` for that and embed); animations are limited to GIFs (no per-line build animations like Beamer's `\pause`); PDF export is decent but the terminal version is the canonical view.

## Similar / related topics

- **slides** (`maaslalani/slides`) — the older Go competitor; simpler, fewer features.
- **patat** — Haskell-based markdown terminal presenter; `pandoc` formats supported.
- **mdp** — minimal C-based markdown presenter (mostly text, no images).
- **Reveal.js / Slidev** — browser-based markdown slides; richer themes, needs a browser.
- **`vhs`** + `asciinema` — for *recording* terminal demos to embed inside slides.

## Internal links

- [[live_demos/README|live_demos]] — Sibling section on terminal recording / demo tooling.
- [[markitdown]] — Markdown-tooling note (unrelated input direction, same ecosystem).

## Keywords

`#presenterm` `#presentations` `#terminal` `#markdown` `#slides` `#kitty` `#mfontanini`

## References / raw notes

- Repo: <https://github.com/mfontanini/presenterm>
- Docs: <https://mfontanini.github.io/presenterm/>
- Demo gif: <https://github.com/mfontanini/presenterm/raw/master/docs/src/assets/demo.gif>

### Install

```shell
brew install presenterm           # macOS / Linuxbrew
cargo install presenterm           # any Rust toolchain
# arch:   pacman -S presenterm
# nix:    nix-shell -p presenterm
```

### Usage

```shell
presenterm slides.md                          # present
presenterm slides.md --export-pdf out.pdf     # export to PDF
presenterm slides.md --present-mode           # speaker mode (notes pane)
```

### Slide source format (taste)

```markdown
---
title: My talk
sub_title: Subtitle
author: Me
theme:
  name: catppuccin-frappe
---

Slide one
===

Standard markdown. Code is syntax-highlighted:

\`\`\`rust
fn main() {
    println!("Hello, presenterm");
}
\`\`\`

<!-- end_slide -->

Slide two
===

Inline image rendered via Kitty graphics:

![diagram](./architecture.png)
```

Slides are separated by `<!-- end_slide -->`; `===`-underlined headings mark slide titles.
