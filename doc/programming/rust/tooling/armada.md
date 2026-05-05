---
title: armada
main_link: https://github.com/resyncgg/armada
keywords: [armada, rust, tcp, scanner]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/tooling/tools.md` by `article_split.py`. Heading: **armada**.

# armada

**Main link:** <https://github.com/resyncgg/armada>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[uclicious]] — UCLicious _(score 17.1)_
- [[loki]] — Loki _(score 17.1)_
- [[starship]] — starship _(score 17.1)_
- [[programming/rust/tooling/tauri|tauri]] — TAURI _(score 17.1)_
- [[rtic]] — RTIC _(score 13.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#armada` `#tooling` `#rust` `#programming` `#tcp` `#scanner` `#syn` `#type`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# armada

TCP scanner

https://github.com/resyncgg/armada

Armada is a high performance TCP SYN scanner. This is equivalent to the type of scanning that nmap might perform when you use the -sS scan type. Armada's main goal is to answer the basic question "Is this port open?". It is then up to you, or your tooling, to dig further to identify what an open port is for.
