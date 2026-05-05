---
title: Helix
main_link: 
keywords: [helix, linux, tools, lsp, tree, sitter, neovim]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Helix

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
- [[fish]] — Fish _(score 24.3)_
- [[tools/linux/zellij|zellij]] — Zellij _(score 24.3)_
- [[cursor]] — Cursor _(score 15.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#helix` `#linux` `#tools` `#lsp` `#tree` `#sitter` `#neovim`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Helix
My Neovim config had 21 external plugins. Making LSP, tree-sitter and formatting work took a while (LSP alone needs 3 plugins) and in the end there were still things that didn’t work.

I’ve switched to Helix, which can do so much out of the box, here’s a non-exhaustive list:

LSP (including autocompletion, show signature, go to definition, show references, etc.) just works
Tree-sitter is built in, you can even do selections on tree-sitter objects
A file picker and global search
Pressing a key in normal mode shows subsequent keys you can press, and what they do
You can jump to any visible word, add/remove/replace quotes or other characters
… and so much more
The config for the code editor I use all day is 5 loc. Here it is:

theme = "kanagawa"

[editor]
line-number = "relative"
cursorline = true
rulers = [80]
I will say that it takes some getting used to as it folows the selection -> action model, i.e. you need to run wd instead of dw to delete the next word.
