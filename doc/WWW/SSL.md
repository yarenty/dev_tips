---
title: HTTPS on websites
main_link: 
keywords: [ssl, certbot, websites, install]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# HTTPS on websites

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[apache]] — apache _(score 16.0)_
- [[sbt]] — SBT _(score 8.2)_
- [[unsloth]] — unsloth _(score 8.2)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#ssl` `#certbot` `#ssls` `#websites` `#install`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# HTTPS on websites

## Certbot 

install:

```shell
sudo apt install certbot python3-certbot-apache
```

run to get/renew SSLs:

```shell
sudo certbot --apache
```
