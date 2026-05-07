---
title: "Typst: a modern, fast LaTeX-shaped typesetting system"
main_link: https://typst.app/
keywords: [typst, typesetting, latex, pdf, documents, scripting]
status: reviewed
---

# Typst: a modern, fast LaTeX-shaped typesetting system

**Main link:** <https://typst.app/>

Repo: <https://github.com/typst/typst> · Docs: <https://typst.app/docs/>

## Summary

[Typst](https://typst.app/) is a markup-based typesetting system written in Rust that aims to be "as powerful as LaTeX while being much easier to learn". You write `.typ` files (markdown-flavoured syntax with a small typed scripting language for layout, computation, and reusable components), and the `typst compile` CLI produces a PDF. Math typesetting, bibliography management, cross-references, and templates are all first-class. There's a [collaborative web editor](https://typst.app/) similar to Overleaf-for-LaTeX, plus a free local CLI.

## Insight

The pitch over LaTeX is mostly:

- **Compile times in milliseconds, not seconds**, thanks to incremental compilation. This makes "edit, see PDF" almost as tight a loop as a Markdown preview.
- **Friendly error messages**, because Typst's compiler was designed in 2023 by people who knew what bad error messages look like, instead of accreted over 40 years.
- **A real scripting language**, typed and lexically-scoped, instead of LaTeX's `\if\else\fi` and `\edef` gymnastics. You can write `let pi = 3.14` and reuse it the way you'd expect.
- **Sensible defaults**: a fresh `.typ` file produces something that looks fine without preamble incantations.

The pitch *for* LaTeX, if you're considering staying:

- The journal templates of every academic field were written 20 years ago in LaTeX. Typst has templates but the ecosystem is much smaller.
- LaTeX has tikz, biblatex, and several decades of Stack Exchange answers for every weird symbol.
- Typst is young — APIs may still shift in minor ways.

For a personal CV, a thesis from scratch, a one-off report, technical notes — pick Typst. For a journal submission, contributing to a paper started in LaTeX, or anywhere you'd benefit from `tikz` — stay LaTeX.

## Similar / related topics

- [LaTeX](https://www.latex-project.org/) — the older, much-larger-ecosystem alternative.
- [[react]] — `react-print-pdf` if your "document" is really a layout problem and you'd rather think in components than typesetting.
- [Pandoc](https://pandoc.org/) — converts almost anything to almost anything (incl. Markdown → PDF via LaTeX, ConTeXt, or wkhtmltopdf).
- [SILE](https://sile-typesetter.org/) — another modern typesetter; even smaller community than Typst.
- [[mdmath_symbols]] — the math-symbol cheat sheet (LaTeX syntax, also accepted by Typst's math mode).

## Internal links

- [[react]] — alternative when "PDF" is really "HTML, but printed"
- [[mdmath_symbols]] — math-symbol cheat sheet usable in either LaTeX or Typst's math mode

## Keywords

`#visualization` `#pdf` `#typst` `#typesetting` `#latex-alternative` `#documents` `#rust`

## References / raw notes

### Project pitch

> Typst is a new markup-based typesetting system that is designed to be as powerful as LaTeX while being much easier to learn and use. Typst has:
>
> - Built-in markup for the most common formatting tasks
> - Flexible functions for everything else
> - A tightly integrated scripting system
> - Math typesetting, bibliography management, and more
> - Fast compile times thanks to incremental compilation
> - Friendly error messages in case something goes wrong

### Hello-world

```typst
= My document title

This is a paragraph with *emphasis* and _italics_.

#let pi = 3.14159
The value of pi is #pi.

== Math
$ integral_0^infinity e^(-x^2) dif x = sqrt(pi) / 2 $

== A figure
#figure(
  image("plot.png", width: 80%),
  caption: [A plot generated externally.],
) <plot>

See @plot for the data.
```

Compile with:

```sh
typst compile my_doc.typ        # produces my_doc.pdf
typst watch my_doc.typ          # live recompile on save
```

### Useful entry points

- Web editor: <https://typst.app/>
- Source: <https://github.com/typst/typst>
- Documentation: <https://typst.app/docs/>
- Community templates: <https://typst.app/universe>
