---
title: Kuasar
main_link: https://github.com/kuasar-io/kuasar
keywords: [kuasar, kubernetes, sandbox, api, container, open]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Kuasar

**Main link:** <https://github.com/kuasar-io/kuasar>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#kuasar` `#kubernetes` `#sandbox` `#api` `#container` `#open`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Kuasar

https://github.com/kuasar-io/kuasar

https://kuasar.io/



_Note (2023/04): The minimum versions of Linux distributions supported by Kuasar are Ubuntu 22.04 or CentOS 8 or openEuler 23.03._

Kuasar is an efficient container runtime that provides cloud-native, all-scenario container solutions by supporting multiple sandbox techniques. Written in Rust, it offers a standard sandbox abstraction based on the sandbox API. Additionally, Kuasar provides an optimized framework to accelerate container startup and reduce unnecessary overheads.

## Why Kuasar?

In the container world, a sandbox is a technique used to separate container processes from each other, and from the operating system itself. After the introduction of the Sandbox API, sandbox has become the first-class citizen in containerd. With more and more sandbox techniques available in the container world, a management service called "sandboxer" is expected to be proposed.

Kuasar supports various types of sandboxers, making it possible for users to select the most appropriate sandboxer for each application, according to application requirements.


Compared with other container runtimes, Kuasar has the following advantages:

- Unified Sandbox Abstraction: The sandbox is a first-class citizen in Kuasar as the Kuasar is entirely built upon the Sandbox API, which was previewed by the containerd community in October 2022. Kuasar fully utilizes the advantages of the Sandbox API, providing a unified way for sandbox access and management, and improving sandbox O&M efficiency.
- Multi-Sandbox Colocation: Kuasar has built-in support for mainstream sandboxes, allowing multiple types of sandboxes to run on a single node. Kuasar is able to balance user's demands for security isolation, fast startup, and standardization, and enables a serverless node resource pool to meet various cloud-native scenario requirements.
- Optimized Framework: Optimization has been done in Kuasar via removing all pause containers and replacing shim processes by a single resident sandboxer process, bringing about a 1:N process management model, which has a better performance than the current 1:1 shim v2 process model. The benchmark test results showed that Kuasar's sandbox startup speed 2x, while the resource overhead for management was reduced by 99%. More details please refer to Performance.
- Open and Neutral: Kuasar is committed to building an open and compatible multi-sandbox technology ecosystem. Thanks to the Sandbox API, it is more convenient and time-saving to integrate sandbox technologies. Kuasar keeps an open and neutral attitude towards sandbox technologies, therefore all sandbox technologies are welcome. Currently, the Kuasar project is collaborating with open-source communities and projects such as WasmEdge, openEuler and QuarkContainers.





![architecture](https://github.com/kuasar-io/kuasar/raw/main/docs/images/arch.png)
