---
title: Programming fonts
main_link: https://github.com/be5invis/Iosevka
keywords: [fonts, programming-fonts, iosevka, fira-code, victor-mono, ligatures, monospace]
status: reviewed
review_date: 2026/05/03
---

# Programming fonts

**Main link:** <https://github.com/be5invis/Iosevka>

## Summary

A short curated list of monospace fonts I've actually used or wanted to use for code. The big axes are:

- **Ligature support** — `=>`, `!=`, `>=`, `->` rendered as a single glyph. Polarising; some people love it, some refuse to use it.
- **Width** — Iosevka and Victor Mono are narrow, Source Code Pro and Space Mono are wider; matters when you have a 100-col rule and a vertical split.
- **Italics shape** — Victor Mono ships handwritten cursive italics, which look fantastic in code with `*comments in italics*` if your editor highlights them that way.

## Insight

Pick a font with **ligatures off by default** when you're new to a codebase — they hide the actual character sequence and that matters when you're grepping or pairing. Once you're comfortable, ligatures genuinely improve scan-ability of `>=` vs `=>` etc.

For long sessions my preference is **Iosevka** (variable-width, dense, hyper-customisable through the build matrix at <https://typeof.net/Iosevka/customizer>), with **Victor Mono** as the "fun font" for slides, demos, and personal projects (the cursive italics make screen-recordings noticeably more readable). **Fira Code** is the safe default if you don't want to think about it — it ships everywhere and the ligature set is well-tuned.

## Similar / related topics

- [JetBrains Mono](https://www.jetbrains.com/lp/mono/) — JetBrains' own; ligatures on by default; ships with their IDEs.
- [Cascadia Code](https://github.com/microsoft/cascadia-code) — Microsoft's; default in Windows Terminal.
- [Hack](https://sourcefoundry.org/hack/) — utilitarian, no ligatures, very legible at small sizes.
- [Berkeley Mono](https://berkeleygraphics.com/typefaces/berkeley-mono/) — paid, beautiful; cited by a lot of designer-developers.
- [Nerd Fonts](https://www.nerdfonts.com/) — patches the above with extra glyphs (file-type icons, powerline arrows) so terminal prompts and file-tree plugins render correctly.

## Internal links

- [[diagrams]] — when you want a font for *labelling* diagrams, not for code
- [[plotters]] — chart text rendering also depends on font choices

## Keywords

`#visualization` `#fonts` `#programming-fonts` `#iosevka` `#fira-code` `#victor-mono` `#ligatures` `#monospace`

## References / raw notes

| Font | Repo / link | Ligatures | Notes |
|------|-------------|-----------|-------|
| **Iosevka** | <https://github.com/be5invis/Iosevka> | yes | narrow, build-your-own variant via the customiser |
| **Fira Code** | <https://github.com/tonsky/FiraCode> | yes | the canonical "ligature font"; safe default |
| **Victor Mono** | <https://github.com/rubjo/victor-mono> | yes | cursive italics; great for screencasts |
| **mononoki** | <https://madmalik.github.io/mononoki/> | no | clean, readable; small project |
| **Source Code Pro** | <https://github.com/adobe-fonts/source-code-pro> | no | Adobe's; wide, very legible |
| **Space Mono** | <https://fonts.google.com/specimen/Space+Mono> | no | Google Fonts; distinctive, slightly retro |
| **JetBrains Mono** | <https://www.jetbrains.com/lp/mono/> | yes | default in JetBrains IDEs |
| **Cascadia Code** | <https://github.com/microsoft/cascadia-code> | yes | default in Windows Terminal |
| **Hack** | <https://sourcefoundry.org/hack/> | no | utilitarian, terminal-friendly |

If your terminal prompt uses [Powerline](https://github.com/powerline/powerline) / [Starship](https://starship.rs/) / similar with file-type glyphs, you almost certainly want a [Nerd Fonts](https://www.nerdfonts.com/) patched build of whichever font above you pick.
