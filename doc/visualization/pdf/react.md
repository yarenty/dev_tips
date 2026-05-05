---
title: React Print
main_link: https://github.com/OnedocLabs/react-print-pdf
keywords: [react, print, components, hello]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# React Print

**Main link:** <https://github.com/OnedocLabs/react-print-pdf>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[typst]] — typst _(score 16.0)_
- [[latex]] — Latex _(score 16.0)_
- [[mdmath_symbols]] — Mdmath Symbols _(score 16.0)_
- [[rocket]] — Rocket _(score 8.9)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#react` `#visualization` `#print` `#components` `#hello` `#pdfs`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# React Print

The new way to build documents.

High-quality, unstyled components for creating PDFs.


https://github.com/OnedocLabs/react-print-pdf



## Key Features 🎯
- Easy to use: Build your first PDF with react-print-pdf in less than 5 minutes.
- Open source: Freedom is beautiful, and so is Onedoc. React-print-pdf is open source and free to use.
- Components & Templates: Kickstart your next document by using our list of components and template created by Onedoc's Team and the community.
- 100% Layout's control: Unlike other solutions, you have complete control over 100% of your layout, including margins, headers, footers, and more.
- Integrate dynamic data to your PDF: Streamline data from your database and integrate it seamlessly into your PDFs.

## Introduction ℹ️
A collection of high-quality, unstyled components for creating beautiful PDFs using React and TypeScript. Forget about docx, latex, or painful outdated libraries. With react-print-pdf, embrace a new way to create PDFs, designed by and for developers.

## Why❓
We believe documents are at the core of communication—invoices, contracts, resumes, brochures, etc. They are the primary method for exchanging information with others professionally. So, why do we continue to use decades-old technology to create them? We believe you deserve better. Document production needs to be modernized. Start today and create your next PDF the same way you build a web app. And yes, this includes automating data integration into your documents. Say hello to react-print-pdf.

## How does it differ from other solutions? 🧐
Unlike other solutions, react-print-pdf gives you complete control over your documents, allowing you to design complex layouts with features like footnotes, headers, margins, and more. Additionally, it enables you to track and analyze specific parts of your document, and build and update charts using data from your database. And this is just the beginning—our team and the community will continue to develop great features to simplify the PDF generation process.


## Getting started 🚀
1. Installation 💿
   Get the react-print component library.

With npm
npm install @onedoc/react-print
With yarn
yarn add @onedoc/react-print
With pnpm
pnpm add @onedoc/react-print

2. Import component ↪️
   Import the components you need to your PDF template from our list of pre-build components :

import { PageTop, PageBottom, PageBreak } from "@onedoc/react-print";

3. Integrate in your document 📄
   Integrate your components and include styles where needed.

export const document = ({ props }) => {
return (
<div>
<PageTop>
<span>Hello #1</span>
</PageTop>
<div>Hello #2</div>
<PageBottom>
<div className="text-gray-400 text-sm">Hello #3</div>
</PageBottom>
<PageBreak />
<span>Hello #4, but on a new page ! </span>
</div>
);
};

## Components 🗂️
A set of standard components to help you build amazing documents without having to deal with the mess of creating complex layouts and maintaining archaic markup. Help us extend this list by actively contributing and adding your favorite components!

[Browse all currently supported components →](https://react.onedoclabs.com/introduction#components)



## Integrations 🔗
PDF designed with react-print-print can be generated, hosted (and more) with your preferred document management providers.

- [Onedoc : HTML to PDF, cloud hosting, analytics and more.](https://www.onedoclabs.com/)
- [Prince XML : simple HTML to PDF tool](https://www.princexml.com/)
- Others (coming soon..)
