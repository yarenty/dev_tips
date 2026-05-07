---
title: "Go"
keywords: [go, golang, cli, network-services, static-binary, goroutines]
status: reviewed
review_date: 2026/05/03
---

# Go

This section is currently sparse — there are no in-depth articles yet. That's not a comment on Go's relevance (it's the lingua franca of cloud-native infrastructure); it just reflects what I've happened to write up here.

## When Go is the right tool

- **CLI tools** — single static binary, fast startup, easy cross-compilation, sane stdlib for argument parsing, JSON, HTTP clients.
- **Network services** — goroutines + channels make I/O-bound concurrent servers genuinely simple. The standard `net/http` is production-grade.
- **Glue and orchestration** — most of the cloud-native ecosystem is Go (Kubernetes, Docker, Terraform, Prometheus, etcd, Consul, NATS server, Cloudflare's edge stack), so writing tools that integrate with it is friction-free.
- **Cross-team code that has to be read by mid-level engineers a year from now** — Go's deliberately-small surface area is a feature here.

## When Go isn't

- CPU-bound numeric / SIMD work (no generics on number types until 1.18, and the codegen story is still behind C++/Rust).
- Anything requiring zero-allocation hot paths (the GC is good, but it's still a GC).
- Rich type-level expressiveness (generics arrived but are intentionally limited; no sum types, no pattern matching).

## Canonical resources

- [A Tour of Go](https://go.dev/tour/) — the official interactive intro.
- [Go by Example](https://gobyexample.com/) — task-oriented snippets, the fastest way to get productive.
- [Effective Go](https://go.dev/doc/effective_go) — official idiom guide.
- [*The Go Programming Language*](https://www.gopl.io/) (Donovan & Kernighan) — the canonical book.
- [Standard library docs](https://pkg.go.dev/std) — Go's stdlib is unusually high-quality; reading it is a learning method in itself.
- [Dave Cheney's blog](https://dave.cheney.net/) — long-running source of "how Go actually works" deep dives.

## Go projects already covered elsewhere in this vault

- [[k3s]] — lightweight Kubernetes distribution.
- [[prometheus]] — metrics server.
- [[node_exporter]] — Prometheus host-metrics exporter.
- [[nats]] — messaging server.
- [[dokku]] — self-hosted PaaS (mix of Go and shell).
- [[mtr]], [[rustscan]] — network tools (rustscan is Rust; comparable Go tools exist).

## See also

- [[programming/package_managers/README|package managers]] — `go mod` is the modern story.
- [[programming/rust/README|Rust]] — the other systems-ish language people compare Go to.
- [[programming/README|programming]] — parent.

## Keywords

`#go` `#golang` `#cli` `#network-services` `#static-binary` `#goroutines` `#programming`
