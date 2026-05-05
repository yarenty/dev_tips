---
title: Akri
main_link: https://github.com/deislabs/akri/blob/main/README.md
keywords: [akri, kubernetes, devices, usb, resources, gpus]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Akri

**Main link:** <https://github.com/deislabs/akri/blob/main/README.md>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[kuasar]] — Kuasar _(score 22.9)_
- [[balena]] — Balena _(score 22.9)_
- [[dokku]] — Dokku _(score 22.9)_
- [[embassy]] — Embassy _(score 20.9)_
- [[k3s]] — K3S _(score 10.9)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#akri` `#kubernetes` `#devices` `#readme` `#usb` `#resources`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Akri

https://github.com/deislabs/akri/blob/main/README.md




Akri lets you easily expose heterogeneous leaf devices (such as IP cameras and USB devices) as resources in a Kubernetes cluster, while also supporting the exposure of embedded hardware resources such as GPUs and FPGAs. Akri continually detects nodes that have access to these devices and schedules workloads based on them.

Simply put: you name it, Akri finds it, you use it.


image::{docdir}/../img/kubernetes/akri-architecture.svg[]
