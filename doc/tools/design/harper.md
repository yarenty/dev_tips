---
title: "Harper — open-source English grammar checker by Automattic"
main_link: https://github.com/Automattic/harper
keywords: [harper, grammar-checker, automattic, english, rust, lsp, privacy, writing]
status: reviewed
review_date: 2026/05/03
---

# Harper — open-source English grammar checker

**Main link:** <https://github.com/Automattic/harper>

Site: <https://writewithharper.com/>

## Summary

Harper is an open-source English grammar checker maintained by Automattic (the WordPress / Tumblr company). It runs **entirely on-device** — no API key, no cloud round-trip, no telemetry — and is fast enough to lint while you type. It ships as a CLI, a Language Server (LSP) for Neovim/VS Code/Zed/Helix, and editor plugins. The author's pitch is that Harper is "just right" — narrower than Grammarly, sharper than `aspell`, and orders of magnitude faster than [LanguageTool](https://languagetool.org/) because it's all native Rust.

## Insight

Reach for Harper when:

- You write a lot of Markdown / docs / comments and want **inline** grammar feedback in your editor.
- You can't or won't ship your prose to a third-party SaaS (NDA work, security policy, or just principle).
- You want lint-level tooling: a CI step that catches `"it's vs its"` in your README.

Comparisons:

- **Grammarly** — better suggestions, slick UI, but cloud-only, paid, and intrusive.
- **LanguageTool** — open-source, very capable, written in Java, runs as a server (heavy). Harper is lighter and faster.
- **Vale** — prose linter focused on style guides (Microsoft, Google, Red Hat); _complementary_ to Harper, not a replacement. Use Vale for "don't use the word `simply`" and Harper for "this sentence has a subject-verb agreement issue".
- **`aspell` / `hunspell`** — spell-check only.

Caveats: English-only (deliberately), still a young project, and the rule set is more conservative than Grammarly's.

## Similar / related topics

- [LanguageTool](https://languagetool.org/) — heavyweight open-source alternative, multi-language.
- [Vale](https://vale.sh/) — style-guide linter; pair with Harper.
- [textlint](https://textlint.github.io/) — JS-based prose linter with a plugin ecosystem.
- [proselint](https://github.com/amperser/proselint) — Python prose linter focused on writing advice from style guides.
- [[markitdown]] — convert proprietary docs to Markdown, then lint with Harper.

## Internal links

- [[markitdown]] — get text into Markdown so Harper / Vale can lint it.
- [[mdfried]] — preview the cleaned-up Markdown in the terminal.
- [[helix]] — Harper has a Helix LSP integration.

## Keywords

`#harper` `#grammar-checker` `#automattic` `#english` `#rust` `#lsp` `#privacy` `#writing` `#design` `#tools`

## References / raw notes

> Harper is an English grammar checker designed to be just right. I created it after years of dealing with the shortcomings of the competition.
>
> <https://github.com/Automattic/harper>
