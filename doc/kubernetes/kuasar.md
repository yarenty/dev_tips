---
title: "Kuasar: Rust container runtime built on the Sandbox API"
main_link: https://kuasar.io/
keywords: [kuasar, container-runtime, sandbox-api, containerd, microvm, wasm, rust, openeuler]
status: reviewed
review_date: 2026/05/03
---

# Kuasar: Rust container runtime built on the Sandbox API

**Main link:** <https://kuasar.io/>

Repo: <https://github.com/kuasar-io/kuasar>

## Summary

[Kuasar](https://kuasar.io/) is a Rust container runtime that sits where you'd normally find a [containerd shim](https://github.com/containerd/containerd/blob/main/runtime/v2/README.md). Instead of one shim process per container (the v2 model), Kuasar runs a **single resident "sandboxer" process** per sandbox type — MicroVM, WasmEdge, application kernel — and multiplexes many containers through it. The result, per the project's own benchmarks, is **~2× faster sandbox startup and ~99% less management overhead** vs. the traditional shim-per-container model.

It's the canonical real-world implementation of the [containerd Sandbox API](https://github.com/containerd/containerd/blob/main/api/services/sandbox/v1/sandbox.proto) (introduced October 2022) and is built primarily by [Huawei](https://www.huawei.com/) for [openEuler](https://www.openeuler.org/).

## Insight

This is a serverless / multi-tenant infrastructure project, not a hobby tool. Reach for Kuasar when:

- You're building a **serverless container platform** and the cold-start latency of spawning a new shim+sandbox per request is hurting you.
- You want to **mix sandbox technologies on the same node** — fire up a regular runC container for a service, a [Kata](https://katacontainers.io/) MicroVM for an untrusted job, and a [WasmEdge](https://wasmedge.org/) WASM runtime for a function — without running three separate shim ecosystems.
- You're running on the supported distros: **Ubuntu 22.04+, CentOS 8+, openEuler 23.03+**. The minimum-version requirement is real because Kuasar leans on newer cgroup v2 / eBPF features.

Don't reach for it when:

- You just want "containers on Kubernetes". The default containerd + runC shim works fine and you don't need the operational complexity of replacing it.
- You don't have a measurable pain point that the Sandbox API solves. The 99% overhead reduction is dramatic in *relative* terms but small in absolute terms unless you're spawning thousands of containers per minute.

The strategic interest is what Kuasar represents: it's the first runtime that treats the **sandbox** as the unit of management instead of the **container**, which lines up with where serverless / FaaS / micro-VM platforms have been pushing for years. If you want to understand the future of container runtimes, read the Kuasar architecture docs even if you never deploy it.

## Similar / related topics

- [containerd](https://containerd.io/) — the upstream runtime Kuasar plugs into via the Sandbox API.
- [Kata Containers](https://katacontainers.io/) — MicroVM runtime; Kuasar can host Kata-style sandboxes.
- [WasmEdge](https://wasmedge.org/) — WASM runtime; Kuasar's natively-supported WASM sandboxer.
- [QuarkContainers](https://github.com/QuarkContainer/Quark) — application-kernel runtime collaborating with Kuasar.
- [youki](https://github.com/youki-dev/youki) — another Rust container project, but at a different layer (low-level OCI runtime, like runC).

## Internal links

- [[k3s]] — typical Kubernetes the underlying containerd is part of
- [[balena]] — different angle on container infra (single-host edge OS)

## Keywords

`#kubernetes` `#kuasar` `#container-runtime` `#sandbox-api` `#containerd` `#microvm` `#wasm` `#rust` `#openeuler` `#serverless`

## References / raw notes

### Pitch (paraphrased from the project README)

> Kuasar is an efficient container runtime that provides cloud-native, all-scenario container solutions by supporting multiple sandbox techniques. Written in Rust, it offers a standard sandbox abstraction based on the sandbox API. Additionally, Kuasar provides an optimized framework to accelerate container startup and reduce unnecessary overheads.

### Stated advantages

- **Unified sandbox abstraction** — sandbox is a first-class citizen; built entirely on the containerd Sandbox API.
- **Multi-sandbox colocation** — runC, Kata MicroVM, WasmEdge, application-kernel sandboxes can all run on the same node.
- **Optimised framework** — removes pause containers, replaces per-container shims with a single resident sandboxer process. Benchmarks: ~2× sandbox startup speedup, ~99% management-overhead reduction.
- **Open and neutral** — actively collaborating with WasmEdge, openEuler, QuarkContainers; no preferred sandbox.

### Minimum versions (per 2023/04 release notes)

The runtime relies on newer kernel and cgroup features:

- Ubuntu 22.04+
- CentOS 8+
- openEuler 23.03+

### Useful entry points

- Project site: <https://kuasar.io/>
- Repo: <https://github.com/kuasar-io/kuasar>
- Architecture diagram: <https://github.com/kuasar-io/kuasar/blob/main/docs/images/arch.png>
- containerd Sandbox API spec: <https://github.com/containerd/containerd/blob/main/api/services/sandbox/v1/sandbox.proto>
