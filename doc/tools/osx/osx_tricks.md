---
title: Jail-break from not opening on OSX
main_link: 
keywords: [osx-tricks, osx, tools, jail, terminal, break]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Jail-break from not opening on OSX

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[osxcross]] — OSX Cross _(score 26.8)_
- [[mdfried]] — Mdfried _(score 21.9)_
- [[byobu]] — Byobu: Text-based Window Manager and Terminal Multiplexer _(score 21.9)_
- [[utm]] — UTM _(score 14.8)_
- [[ios]] — Ios _(score 14.8)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#osx-tricks` `#osx` `#tools` `#jail` `#terminal` `#remove` `#break`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Jail-break from not opening on OSX


1. After downloading, open Terminal

Remove quarantine attribute:
```bash
sudo xattr -r -d com.apple.quarantine /Applications/Nuclear.app
```

2. If still blocked:

```log

Go to System Preferences → Security & Privacy
Click "Open Anyway" next to Nuclear
```

3. For persistent issues:

```bash 
sudo spctl --master-disable
```
