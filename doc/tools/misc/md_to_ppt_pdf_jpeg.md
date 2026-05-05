---
title: MARP
main_link: https://github.com/marp-team/marp-cli
keywords: [md-to-ppt-pdf-jpeg, misc, tools, pptx, slide, deck, images]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# MARP

**Main link:** <https://github.com/marp-team/marp-cli>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#md-to-ppt-pdf-jpeg` `#misc` `#tools` `#pptx` `#slide` `#deck` `#images`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# MARP

https://github.com/marp-team/marp-cli


marp slide-deck.md -o output.html


## PDF
marp --pdf slide-deck.md
marp slide-deck.md -o converted.pdf


## PPT

marp --pptx slide-deck.md
marp slide-deck.md -o converted.pptx

A created PPTX includes rendered Marp slide pages and the support of Marpit presenter notes. It can open with PowerPoint, Keynote, Google Slides, LibreOffice Impress, and so on...



[EXPERIMENTAL] Generate editable pptx (--pptx-editable)
A converted PPTX usually consists of pre-rendered background images, that is meaning contents cannot to modify or re-use in PowerPoint. If you want to generate editable PPTX for modifying texts, shapes, and images in GUI, you can pass --pptx-editable option together with --pptx option.

marp --pptx --pptx-editable slide-deck.md



## PNG/JPEG image(s) 🌐
Multiple images (--images)
You can convert the slide deck into multiple images when specified --images [png|jpeg] option.

Convert into multiple JPEG image files

marp --images jpeg slide-deck.md
