---
title: "Marp — Markdown → PDF / PPTX / HTML / images slide decks"
main_link: https://github.com/marp-team/marp-cli
keywords: [marp, slides, markdown, pptx, pdf, presentation, cli, node]
status: reviewed
---

# Marp — Markdown → PDF / PPTX / HTML / images

**Main link:** <https://github.com/marp-team/marp-cli>

Site: <https://marp.app/> · VS Code extension: <https://marketplace.visualstudio.com/items?itemName=marp-team.marp-vscode>

## Summary

Marp is a Markdown-driven slide-deck toolchain. The CLI (`marp`, Node.js / single-binary) takes a Markdown file with a `marp: true` frontmatter and renders it to **HTML, PDF, PPTX (including editable PPTX), or PNG/JPEG image sequences**. The Marpit syntax extends Markdown with `---` slide separators, per-slide directives, presenter notes, themes, and built-in CSS theming. There's also a Marp VS Code extension and a desktop preview app.

## Insight

This is the tool for engineers who think slide decks should live in Git, diff cleanly, and not require Keynote or PowerPoint to author. You write Markdown like you would for a README, sprinkle a few `<!-- _class: invert -->` directives, and ship a deck.

Reach for it when:

- You're giving an internal engineering talk and you'd rather edit Markdown in your IDE than wrestle Keynote.
- You need the **same content** as PDF (for handouts) AND PPTX (for the corporate AV system) AND HTML (for the website).
- You want slides under version control with diffable changes.
- You want PNG/JPEG exports for embedding individual slides in docs.

Comparisons:

- vs **Reveal.js** — Reveal is HTML-first and more interactive (animations, fragments), but you write more boilerplate. Marp is plainer, faster to author.
- vs **Slidev** — Slidev (Vue, Anthony Fu) is the prettier modern alternative with better animations and dev-server live reload. If you're already in a Node ecosystem and want polish, Slidev wins; if you want a single CLI that exports to PPTX cleanly, Marp wins.
- vs **`presenterm`** — terminal-native; same Markdown-first idea but renders inside the terminal, not to PDF.
- vs **Keynote / PowerPoint** — Marp wins on diff-ability, loses on visual polish and animations.

Caveats: PPTX exports are **rendered as images** by default (so the recipient can't edit text in PowerPoint); use `--pptx --pptx-editable` for an editable PPTX, but expect imperfect fidelity. PDF rendering uses headless Chromium under the hood — first run downloads it.

```bash
marp slide-deck.md -o output.html
marp --pdf slide-deck.md
marp slide-deck.md -o converted.pdf

marp --pptx slide-deck.md
marp slide-deck.md -o converted.pptx

marp --pptx --pptx-editable slide-deck.md   # experimental editable PPTX

marp --images jpeg slide-deck.md             # one .jpeg per slide
```

## Similar / related topics

- [Reveal.js](https://revealjs.com/) — JS-driven HTML slides; richer interactivity.
- [Slidev](https://sli.dev/) — Vue-based developer slides with live coding blocks.
- [`presenterm`](https://github.com/mfontanini/presenterm) — Markdown slides _inside_ the terminal (see [[term]]).
- [Pandoc + Beamer](https://pandoc.org/) — LaTeX route to PDF slides.
- [[typst]] — typesetting alternative for high-fidelity PDFs.

## Internal links

- [[mdfried]] — preview the Markdown source before exporting.
- [[markitdown]] — pull existing PPTX content into Markdown to migrate decks.
- [[typst]] — when you outgrow Marp's CSS theming.
- [[ascii_generator]] — quick banner art for slide section dividers.

## Keywords

`#marp` `#slides` `#markdown` `#pptx` `#pdf` `#presentation` `#cli` `#node` `#misc` `#tools`

## References / raw notes

```bash
marp slide-deck.md -o output.html
```

### PDF

```bash
marp --pdf slide-deck.md
marp slide-deck.md -o converted.pdf
```

### PPTX

```bash
marp --pptx slide-deck.md
marp slide-deck.md -o converted.pptx
```

A created PPTX includes rendered Marp slide pages and supports Marpit presenter notes. Opens in PowerPoint, Keynote, Google Slides, LibreOffice Impress, etc.

**[EXPERIMENTAL] Editable PPTX (`--pptx-editable`)**

A converted PPTX usually consists of pre-rendered background images, meaning content can't be modified or re-used in PowerPoint. Pass `--pptx-editable` together with `--pptx` to generate an editable PPTX:

```bash
marp --pptx --pptx-editable slide-deck.md
```

### PNG / JPEG image(s)

Convert the deck into multiple images with `--images [png|jpeg]`:

```bash
marp --images jpeg slide-deck.md
```
