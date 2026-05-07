---
title: Markdown / LaTeX math symbol cheat sheet
main_link: https://katex.org/docs/supported.html
keywords: [mdmath, latex, math, symbols, cheat-sheet, mathjax, katex]
status: reviewed
review_date: 2026/05/03
---

# Markdown / LaTeX math symbol cheat sheet

**Main link:** <https://katex.org/docs/supported.html>

## Summary

A practical lookup table for the LaTeX-flavoured math syntax used by [KaTeX](https://katex.org/), [MathJax](https://www.mathjax.org/), GitHub-flavoured Markdown's `$...$` / `$$...$$` blocks, Obsidian, mdbook (with the `mathjax-support` setting), and Typst's math mode (which accepts most of the same names). Inline math goes between single dollars (`$\alpha$`), display math between double dollars or in a `$$ ... $$` block.

## Insight

You don't need to memorise these — bookmark the table below or [the canonical KaTeX list](https://katex.org/docs/supported.html). The only thing worth remembering is the *shape* of the syntax:

- Greek letters: `\alpha` (lowercase) / `\Alpha` (capital, where it differs visually from Latin); `\varphi` is the cursive variant of `\phi`.
- Sub/super: `x_n`, `x^2`, `x_{n+1}`, `x^{2k}`. Always brace anything multi-character.
- Fractions: `\frac{n!}{k!(n-k)!}` and the binomial sibling `\binom{n}{k}`.
- Sums / integrals: `\sum_{i=1}^{N}`, `\int_0^\infty`, `\prod`, `\bigcup`, `\bigcap`. Use `\substack{0<i<m \\ 0<j<n}` to stack subscripts.
- Decorations: `\hat{a}`, `\bar{a}`, `\vec{a}`, `\overline{abc}`, `\overrightarrow{AB}`.
- Brackets: `\langle ... \rangle`, `\lfloor ... \rfloor`, `\lceil ... \rceil`. Use `\left( ... \right)` for auto-sizing.

If a renderer rejects a symbol, check whether it's KaTeX-specific or MathJax-specific — KaTeX deliberately implements a *subset* of LaTeX math for speed, so `\substack` and a few other things only work in MathJax.

## Similar / related topics

- [KaTeX supported functions](https://katex.org/docs/supported.html) — the canonical, exhaustive list.
- [MathJax supported TeX](https://docs.mathjax.org/en/latest/input/tex/macros/index.html) — superset of KaTeX.
- [Detexify](http://detexify.kirelabs.org/classify.html) — *draw* a symbol, get the LaTeX command.
- [[typst]] — Typst's math mode accepts most of the same names plus its own ergonomic shorthand.
- [[react]] — `react-print-pdf` can embed KaTeX-rendered math in HTML PDFs.

## Internal links

- [[typst]] — alternative rendering target that also speaks LaTeX-shaped math
- [[react]] — embed math in HTML-shaped PDFs

## Keywords

`#visualization` `#pdf` `#mdmath` `#latex` `#math` `#symbols` `#cheat-sheet` `#katex` `#mathjax`

## References / raw notes

### Greek letters

| Symbol | Script |
|--------|--------|
| $\alpha$ | `\alpha` |
| $A$ | `A` (uppercase alpha is just `A`) |
| $\beta$ | `\beta` |
| $B$ | `B` |
| $\gamma$ | `\gamma` |
| $\Gamma$ | `\Gamma` |
| $\pi$ | `\pi` |
| $\Pi$ | `\Pi` |
| $\phi$ | `\phi` |
| $\Phi$ | `\Phi` |
| $\varphi$ | `\varphi` |
| $\theta$ | `\theta` |

### Operators

| Symbol | Script |
|--------|--------|
| $\cos$ | `\cos` |
| $\sin$ | `\sin` |
| $\lim$ | `\lim` |
| $\exp$ | `\exp` |
| $\to$ | `\to` |
| $\infty$ | `\infty` |
| $\equiv$ | `\equiv` |
| $\bmod$ | `\bmod` |
| $\times$ | `\times` |

### Powers and indices

| Symbol | Script |
|--------|--------|
| $k_{n+1}$ | `k_{n+1}` |
| $n^2$ | `n^2` |
| $k_n^2$ | `k_n^2` |

### Fractions and binomials

| Symbol | Script |
|--------|--------|
| $\frac{n!}{k!(n-k)!}$ | `\frac{n!}{k!(n-k)!}` |
| $\binom{n}{k}$ | `\binom{n}{k}` |
| $\frac{\frac{x}{1}}{x - y}$ | `\frac{\frac{x}{1}}{x - y}` |
| $^3/_7$ | `^3/_7` |

### Roots

| Symbol | Script |
|--------|--------|
| $\sqrt{k}$ | `\sqrt{k}` |
| $\sqrt[n]{k}$ | `\sqrt[n]{k}` |

### Sums, products, integrals

| Symbol | Script |
|--------|--------|
| $\sum_{i=1}^{10} t_i$ | `\sum_{i=1}^{10} t_i` |
| $\int_0^\infty \mathrm{e}^{-x}\,\mathrm{d}x$ | `\int_0^\infty \mathrm{e}^{-x}\,\mathrm{d}x` |
| $\sum$ | `\sum` |
| $\prod$ | `\prod` |
| $\coprod$ | `\coprod` |
| $\bigoplus$ | `\bigoplus` |
| $\bigotimes$ | `\bigotimes` |
| $\bigodot$ | `\bigodot` |
| $\bigcup$ | `\bigcup` |
| $\bigcap$ | `\bigcap` |
| $\biguplus$ | `\biguplus` |
| $\bigsqcup$ | `\bigsqcup` |
| $\bigvee$ | `\bigvee` |
| $\bigwedge$ | `\bigwedge` |
| $\int$ | `\int` |
| $\oint$ | `\oint` |
| $\iint$ | `\iint` |
| $\iiint$ | `\iiint` |
| $\idotsint$ | `\idotsint` |
| $\sum_{\substack{0<i<m \\ 0<j<n}} P(i,j)$ | `\sum_{\substack{0<i<m \\ 0<j<n}} P(i,j)` (MathJax only) |
| $\int\limits_a^b$ | `\int\limits_a^b` |

### Decorations / accents

| Symbol | Script |
|--------|--------|
| $a'$, $a^{\prime}$ | `a'`, `a^{\prime}` |
| $a''$ | `a''` |
| $a'''$ | `a'''` |
| $\hat{a}$ | `\hat{a}` |
| $\bar{a}$ | `\bar{a}` |
| $\grave{a}$ | `\grave{a}` |
| $\acute{a}$ | `\acute{a}` |
| $\dot{a}$ | `\dot{a}` |
| $\ddot{a}$ | `\ddot{a}` |
| $\not{a}$ | `\not{a}` |
| $\mathring{a}$ | `\mathring{a}` |
| $\overrightarrow{AB}$ | `\overrightarrow{AB}` |
| $\overleftarrow{AB}$ | `\overleftarrow{AB}` |
| $\overline{aaa}$ | `\overline{aaa}` |
| $\check{a}$ | `\check{a}` |
| $\vec{a}$ | `\vec{a}` |
| $\underline{a}$ | `\underline{a}` |
| $\color{red}x$ | `\color{red}x` |

### Spacing & ellipses

| Symbol | Script |
|--------|--------|
| $\pm$ | `\pm` |
| $\mp$ | `\mp` |
| $\int y\,\mathrm{d}x$ | `\int y\,\mathrm{d}x` (note `\,` — thin space) |
| `,` `:` `;` `!` | comma / colon / semicolon / negative thin space |
| $\dots$ | `\dots` |
| $\ldots$ | `\ldots` |
| $\cdots$ | `\cdots` |
| $\vdots$ | `\vdots` |
| $\ddots$ | `\ddots` |

### Brackets

| Symbol | Script |
|--------|--------|
| $(a)$ | `(a)` |
| $[a]$ | `[a]` |
| $\{a\}$ | `\{a\}` (escape the braces!) |
| $\langle f \rangle$ | `\langle f \rangle` |
| $\lfloor f \rfloor$ | `\lfloor f \rfloor` |
| $\lceil f \rceil$ | `\lceil f \rceil` |
| $\ulcorner f \urcorner$ | `\ulcorner f \urcorner` |

For auto-sizing, wrap brackets with `\left` and `\right`:

```latex
\left( \frac{a}{b} \right)
```
