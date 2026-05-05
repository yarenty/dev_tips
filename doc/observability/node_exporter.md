---
title: node_exporter
main_link: 
keywords: [node-exporter, observability, node, exporter, brew, services]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# node_exporter

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#node-exporter` `#observability` `#node` `#exporter` `#brew` `#services`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# node_exporter

When run from `brew services`, `node_exporter` is run from
`node_exporter_brew_services` and uses the flags in:
/usr/local/etc/node_exporter.args

To start node_exporter now and restart at login:
brew services start node_exporter
Or, if you don't want/need a background service you can just run:
/usr/local/opt/node_exporter/bin/node_exporter_brew_services
