---
title: lychee — async link checker for Markdown / HTML
main_link: https://github.com/lycheeverse/lychee
keywords: [lychee, rust, link-checker, markdown, html, ci, github-action]
status: reviewed
---

# lychee — async link checker for Markdown / HTML

**Main link:** <https://github.com/lycheeverse/lychee>

## Summary

`lychee` is a fast, async, stream-based link checker by Matthias Endler. It scans Markdown, HTML, reStructuredText, source files (extracts URLs from comments), or whole websites, and reports broken hyperlinks and email addresses. It's available as a CLI binary, a Rust library, and an [official GitHub Action](https://github.com/lycheeverse/lychee-action) — the GitHub Action is how most teams use it.

## Insight

Reach for `lychee` whenever you have a content site / docs / vault and want CI to fail when an external link 404s. Common patterns:

```sh
# Check all markdown in current dir, recursive
lychee './**/*.md'

# Treat HTTP 429 as success (rate-limited but reachable)
lychee --accept '200,429' ./README.md

# Skip private / known-flaky hosts
lychee --exclude 'localhost|example\.com' ./docs/

# Cache results to avoid re-hitting the same URLs
lychee --cache --max-cache-age 1d ./README.md
```

Configuration lives at `lychee.toml` next to your sources; the GitHub Action posts a Markdown summary table and (optionally) auto-files an issue.

**Gotchas**: by default lychee follows redirects and will hammer your CI minutes if the target site rate-limits — set `--cache` and `--max-concurrency` to be polite. It does not understand JavaScript-rendered pages (no headless browser); if your site is SPA-only, point lychee at the *built* HTML, not the source. For internal-link-only checking inside an mdBook/Hugo/Jekyll site, the built-in tools (`mdbook-linkcheck`, `htmltest`) are usually faster — use lychee for the *external* sweep.

Compared to siblings: **`markdown-link-check`** (Node) is the most-cited alternative but is single-threaded and slow; **`linkchecker`** (Python) is comprehensive but heavyweight; **`htmltest`** (Go) is fast for static-site outputs; **`mdbook-linkcheck`** is the natural pairing for mdBook projects. Pick `lychee` for cross-project Markdown CI; pick `htmltest` if you only need to check built HTML.

## Similar / related topics

- [`markdown-link-check`](https://github.com/tcort/markdown-link-check) — Node, single-threaded, widely used.
- [`linkchecker`](https://linkchecker.github.io/linkchecker/) — Python, full-website crawler.
- [`htmltest`](https://github.com/wjdp/htmltest) — Go, fast for built static-site HTML.
- [`mdbook-linkcheck`](https://github.com/Michael-F-Bryan/mdbook-linkcheck) — mdBook-specific, internal-link focus.
- [`lychee-action`](https://github.com/lycheeverse/lychee-action) — official GitHub Action wrapper.

## Internal links

- [[README]] — tooling section landing.

## Keywords

`#lychee` `#rust` `#link-checker` `#markdown` `#html` `#ci` `#github-action` `#docs`

## References / raw notes

- Repo: <https://github.com/lycheeverse/lychee>
- Crate: <https://crates.io/crates/lychee>
- GitHub Action: <https://github.com/lycheeverse/lychee-action>
- Install: `cargo install lychee` or `brew install lychee`.
