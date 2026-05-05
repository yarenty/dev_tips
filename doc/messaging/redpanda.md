---
title: redpanda
main_link: https://docs.redpanda.com
keywords: [redpanda, messaging, rpk, start, cluster, keeper]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# redpanda

**Main link:** <https://docs.redpanda.com>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#redpanda` `#messaging` `#rpk` `#start` `#cluster` `#keeper`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# redpanda

Redpanda Keeper (rpk) is Redpanda's command line interface (CLI)
utility. The rpk commands let you configure, manage, and tune
Redpanda clusters. They also let you manage topics, groups,
and access control lists (ACLs).
Start a three-node docker cluster locally:

    rpk container start -n 3

Interact with the cluster using commands like:

    rpk topic list

When done, stop and delete the docker cluster:

    rpk container purge

For more examples and guides, visit: https://docs.redpanda.com

fish completions have been installed to:
/usr/local/share/fish/vendor_completions.d
