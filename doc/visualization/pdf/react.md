---
title: "react-print-pdf: build PDFs as React components"
main_link: https://github.com/OnedocLabs/react-print-pdf
keywords: [react-print-pdf, react, pdf, components, onedoc, html-to-pdf]
status: reviewed
---

# react-print-pdf: build PDFs as React components

**Main link:** <https://github.com/OnedocLabs/react-print-pdf>

Author/SaaS: <https://www.onedoclabs.com/>

## Summary

[`react-print-pdf`](https://github.com/OnedocLabs/react-print-pdf) is an Apache-2.0 React + TypeScript component library for assembling PDFs the same way you assemble web pages — `<PageTop>`, `<PageBottom>`, `<PageBreak>`, plus your own components — and rendering the result via headless Chromium (or a hosted service like [Onedoc](https://www.onedoclabs.com/) or [Prince XML](https://www.princexml.com/)). Aimed at developers who'd rather think in HTML/CSS/React than in `\LaTeX` macros or `docx` XML.

## Insight

Reach for this when:

- Your "PDF" is really an *invoice / contract / résumé / brochure* — a document with structured data, dynamic fields, and a layout that's mostly boxes and text. The thing where "build it like a webpage and print it" is the obvious answer.
- You already have a React/TypeScript front-end team and don't want them to context-switch into a separate typesetting language.
- You want **dynamic data integration** (pull from your database into the PDF template) and would rather express that as `{props.invoice.lineItems.map(...)}` than as a Jinja-style template.

Don't reach for this when:

- You need precise typesetting (kerning, microtype, complex math, multi-column journal layouts) — use [[typst]] or LaTeX.
- You need offline / on-device PDF generation without spawning a Chromium — use a server-side library (Puppeteer-free routes are limited; consider [`pdfkit`](https://github.com/foliojs/pdfkit) directly, or [WeasyPrint](https://weasyprint.org/) on the Python side).
- The PDF is the *only* output and there's no React in your stack — you'd be pulling in a node toolchain just for this.

The "Onedoc" piece in the name refers to [OnedocLabs](https://www.onedoclabs.com/), a hosted service from the same team that takes your React component and renders + hosts the PDF for you. The library works fine without it (you bring your own Puppeteer / Prince XML), but it's where the company's revenue comes from.

## Similar / related topics

- [@react-pdf/renderer](https://react-pdf.org/) — older React-to-PDF library; renders to PDF directly *without* a browser engine, which is faster but supports a smaller subset of CSS.
- [Puppeteer](https://pptr.dev/) — headless Chrome; the lower-level "render any HTML to PDF" tool that `react-print-pdf` ultimately sits on top of.
- [Prince XML](https://www.princexml.com/) — commercial HTML-to-PDF engine; the alternative back-end the library mentions.
- [WeasyPrint](https://weasyprint.org/) — Python-side equivalent for HTML-to-PDF.
- [[typst]] — when you'd rather think in typesetting than in HTML/CSS.

## Internal links

- [[typst]] — alternative when you want a typesetting language instead of HTML/CSS
- [[mdmath_symbols]] — math-symbol cheat sheet (LaTeX/MathJax/KaTeX, also embeddable in HTML PDFs)

## Keywords

`#visualization` `#pdf` `#react` `#react-print-pdf` `#components` `#onedoc` `#html-to-pdf`

## References / raw notes

### Project pitch (from the repo README)

> A collection of high-quality, unstyled components for creating beautiful PDFs using React and TypeScript. Forget about docx, latex, or painful outdated libraries.

Key features (per the project):

- Easy to use (claimed first PDF in <5 minutes).
- Open source (Apache 2.0).
- Components & templates from the Onedoc team and the community.
- 100% layout control (margins, headers, footers, ...).
- Dynamic data integration into the PDF.

### Quickstart

```sh
npm install @onedoc/react-print     # or yarn add / pnpm add
```

```tsx
import { PageTop, PageBottom, PageBreak } from "@onedoc/react-print";

export const MyDocument = ({ props }) => (
  <div>
    <PageTop>
      <span>Hello #1</span>
    </PageTop>
    <div>Hello #2</div>
    <PageBottom>
      <div className="text-gray-400 text-sm">Hello #3</div>
    </PageBottom>
    <PageBreak />
    <span>Hello #4, but on a new page!</span>
  </div>
);
```

### Component catalogue + integrations

- All currently supported components: <https://react.onedoclabs.com/introduction#components>
- Onedoc cloud (HTML-to-PDF + hosting + analytics): <https://www.onedoclabs.com/>
- Prince XML alternative back-end: <https://www.princexml.com/>
