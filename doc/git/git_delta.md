---
title: delta
main_link: https://github.com/dandavison/delta
keywords: [git-delta, git, dandavison, brew]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# delta

**Main link:** <https://github.com/dandavison/delta>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[config]] — GIT configuration _(score 23.8)_
- [[github]] — Github README _(score 23.8)_
- [[prometheus]] — prometheus _(score 8.9)_
- [[workmux]] — Workmux _(score 7.8)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#git-delta` `#git` `#dandavison` `#brew` `#install` `#installation`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# delta


brew install git-delta


https://github.com/dandavison/delta


https://dandavison.github.io/delta/installation.html


```bash
git config --global core.pager delta
git config --global interactive.diffFilter 'delta --color-only'
git config --global delta.navigate true
git config --global merge.conflictStyle zdiff3
```
