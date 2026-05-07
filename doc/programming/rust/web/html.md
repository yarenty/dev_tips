---
title: kuchiki (HTML/XML tree manipulation)
main_link: https://crates.io/crates/kuchiki
keywords: [kuchiki, html, xml, scraping, css-selectors, rust, html5ever]
status: reviewed
---

# kuchiki (HTML/XML tree manipulation)

**Main link:** <https://crates.io/crates/kuchiki>

## Summary

`kuchiki` (朽木, "rotten tree") is a CSS-selector-based HTML/XML DOM-like tree library for Rust, built on top of Servo's `html5ever` parser. It exposes a `NodeRef` tree you can walk, mutate, and query with CSS selectors — the closest Rust gets to a `BeautifulSoup`/`jQuery`-style ergonomic experience. Originally by Simon Sapin (Servo team).

## Insight

Note up front: **`kuchiki` is unmaintained** (last release 2020) and most of the ecosystem has moved on to `scraper` (CSS selectors) or `kuchikiki` (a maintained kuchiki fork). Use this article as background; for new code reach for one of those.

The HTML-parsing layer cake:

| Layer | Crate | Use when |
|---|---|---|
| Raw tokeniser/parser | `html5ever` / `xml5ever` | You need every byte of standards compliance, building your own DOM. |
| DOM-like tree + CSS selectors | `scraper` (recommended), `kuchikiki`, `kuchiki` (legacy) | Scraping, extraction, light mutation. |
| jQuery-style chained API | `nipper`, `select` (CSS3 selectors only) | Familiarity from JS; thin wrappers over the above. |
| Headless browser | `fantoccini`, `chromiumoxide`, `thirtyfour` | The page needs JavaScript executed first. |

Gotchas:

- **Selector engine quirks.** All of these use the `selectors` crate from Servo; pseudo-class support (`:has()`, `:nth-of-type`) is more complete than you'd expect, but a few niche selectors silently no-op.
- **Encoding.** HTML in the wild is rarely UTF-8 only; pair with `encoding_rs` and the `html5ever` charset detection if you're scraping arbitrary sites.
- **Mutation.** `kuchiki`/`scraper` trees are shared via `Rc<RefCell<...>>`; cloning a `NodeRef` shares interior state — easy to be surprised.
- **Performance.** For large documents, `lol_html` (Cloudflare's streaming rewriter) is dramatically faster than tree-based libraries when you only need to filter/transform.

## Similar / related topics

- **`scraper`** — the maintained CSS-selector scraping crate; what most new code uses.
- **`kuchikiki`** — actively-maintained fork of `kuchiki` with the same API.
- **`html5ever`** — the underlying spec-compliant parser from Servo.
- **`lol_html`** — Cloudflare's streaming HTML rewriter (very fast, push-based).
- **`scraper` + `fantoccini`** — pair for JS-rendered pages via WebDriver.

## Internal links
- [[reqwest]] — the typical fetcher you pair with an HTML parser
- [[parsers]] — sibling parser crates in the io section
- [[programming/rust/web/README|web README]]

## Keywords

`#kuchiki` `#html` `#xml` `#scraping` `#css-selectors` `#rust` `#html5ever`

## References / raw notes

- Crate: <https://crates.io/crates/kuchiki>
- Repo: <https://github.com/kuchiki-rs/kuchiki> (archived; see `kuchikiki` fork)
- Maintained fork: <https://crates.io/crates/kuchikiki>
- Recommended modern alternative: <https://crates.io/crates/scraper>

> 朽木 — HTML/XML tree manipulation library
