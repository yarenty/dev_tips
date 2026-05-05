---
title: Bandwhich
main_link: https://crates.io/crates/bandwhich
keywords: [bandwhich, rust, cli, cross, terminal, linux]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/programming/rust/tooling/tools.md` by `article_split.py`. Heading: **Bandwhich**.

# Bandwhich

**Main link:** <https://crates.io/crates/bandwhich>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[clido]] — clido _(score 22.7)_
- [[programming/rust/tooling/bottom|bottom]] — bottom _(score 20.6)_
- [[turmoil]] — turmoil _(score 17.1)_
- [[starship]] — starship _(score 17.1)_
- [[debug]] — Debug _(score 17.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#bandwhich` `#tooling` `#rust` `#programming` `#crates` `#cli` `#displaying` `#network`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Bandwhich

https://crates.io/crates/bandwhich

This is a CLI utility for displaying current network utilization by process, connection and remote IP/hostname

How does it work?
bandwhich sniffs a given network interface and records IP packet size, cross referencing it with the /proc filesystem on linux, lsof on macOS, or using WinApi on windows. It is responsive to the terminal window size, displaying less info if there is no room for it. It will also attempt to resolve ips to their host name in the background using reverse DNS on a best effort basis.
