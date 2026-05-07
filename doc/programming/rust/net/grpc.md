---
title: "gRPC in Rust (tonic, grpc-rs)"
main_link: https://github.com/hyperium/tonic
keywords: [rust, grpc, tonic, hyper, http2, protobuf, rpc, raft]
status: reviewed
---

# gRPC in Rust (tonic, grpc-rs)

**Main link:** <https://github.com/hyperium/tonic>

## Summary

[**tonic**](https://github.com/hyperium/tonic) is the de-facto Rust gRPC implementation, built on top of `hyper` (HTTP/2) and `tokio` (async runtime), maintained as part of the [Hyperium](https://github.com/hyperium) project. It generates client and server code from `.proto` files via `tonic-build` (which wraps `prost`). The main alternative is [**grpc-rs**](https://github.com/tikv/grpc-rs) — TiKV's wrapper around the C++ gRPC core, used inside TiKV / TiDB itself but rarely picked for new code.

## Insight

Pick **tonic** for almost every new Rust gRPC project. It's pure Rust, integrates cleanly with the rest of the tokio / tower ecosystem, and the generated code is idiomatic async/await. Reach for **grpc-rs** only when you specifically want feature parity with the C++ core (load balancing policies, certain interceptors, EOL-grade compatibility) and don't mind a C++ build dep.

How tonic slots vs. neighbours:

- **vs. Go gRPC reference impl** ([grpc-go](https://github.com/grpc/grpc-go)) — the Go impl is what most service meshes (Linkerd's old proxy excepted) and Google internal code targets. tonic is wire-compatible — same Protobuf, same HTTP/2 — so cross-language interop is a non-issue. Performance is roughly comparable; tonic edges ahead on memory.
- **vs. [`volo`](https://github.com/cloudwego/volo) (CloudWeGo / ByteDance)** — Rust RPC framework with `volo-grpc`. Pitched as faster than tonic in their benchmarks; smaller ecosystem, ByteDance-driven. Worth considering for raw throughput.
- **vs. [`tarpc`](https://github.com/google/tarpc)** — Google's Rust-only RPC framework. Not gRPC wire-compatible; just a Rust↔Rust RPC. Pick tarpc if both ends are Rust and you don't need cross-language. Pick tonic for everything else.
- **vs. plain HTTP / REST + JSON** — gRPC wins on streaming (4 modes), schema enforcement, and codegen; loses on debuggability (binary wire), browser support (needs grpc-web proxy), and CDN/edge tooling. For internal service-to-service: gRPC. For external API: probably REST or [Connect](https://connectrpc.com/) (gRPC-compatible but works over HTTP/1.1 + JSON too — first-class Rust support coming via `tonic-web` / planned).
- **vs. [tRPC](trpc.md)** — entirely different. tRPC is TypeScript-only API typing for full-stack TS apps. Not a gRPC competitor.

Gotchas:

1. **Build needs `protoc`.** `tonic-build` shells out to the Protobuf compiler at build time. Either install it (`brew install protobuf` / `apt install protobuf-compiler`) or use the `protoc-bin-vendored` crate.
2. **Streaming back-pressure.** `tonic::Streaming` doesn't apply tokio-style back-pressure naturally — large server streams can OOM the client if it can't keep up. Bound your `mpsc::channel` capacities.
3. **Reflection / health are separate crates.** `tonic-reflection`, `tonic-health` — easy to forget when you wonder why `grpcurl -ls` returns nothing.
4. **No native HTTP/1.1 fallback.** Browsers need [`tonic-web`](https://github.com/hyperium/tonic/tree/master/tonic-web) (a grpc-web layer); production deployments often run a Connect-shaped layer in front.

## Similar / related topics

- [`prost`](https://github.com/tokio-rs/prost) — Protobuf code generator that tonic uses; can be used standalone for non-RPC Protobuf.
- [`volo-grpc`](https://github.com/cloudwego/volo) — ByteDance's Rust RPC framework with gRPC support; aggressive perf focus.
- [`tarpc`](https://github.com/google/tarpc) — Google's Rust-native RPC; not gRPC-compatible but easier when both ends are Rust.
- [Apache Arrow Flight](https://arrow.apache.org/docs/format/Flight.html) — gRPC-based service for shipping Arrow record batches; the modern alternative to JDBC/ODBC for analytics.
- [Connect](https://connectrpc.com/) — gRPC-compatible protocol that also works over HTTP/1.1 + JSON; great for browser-facing APIs.
- [Grip](https://gripgrpc.dev/) — native macOS gRPC client (the Postman of gRPC).

## Internal links

<!-- reviewed -->

- [[programming/rust/net/README|Rust net]] — section landing page.
- [[programming/rust/net/trpc|tRPC]] — sibling article — *not* a gRPC alternative, despite the name; clarification lives there.
- [[programming/rust/concurrency/tokio|tokio]] — the runtime tonic builds on.
- [[programming/rust/web/README|Rust web]] — sibling section for HTTP/REST.

## Keywords

`#rust` `#grpc` `#tonic` `#hyper` `#http2` `#protobuf` `#rpc`

## References / raw notes

### Awesome list

<https://github.com/grpc-ecosystem/awesome-grpc>

### tonic — currently most used

<https://github.com/hyperium/tonic>

> A rust implementation of gRPC, a high performance, open source, general RPC framework that puts mobile and HTTP/2 first.

`tonic` is gRPC over HTTP/2 focused on performance, interoperability, and flexibility, with first-class async/await support; designed as a core building block for production Rust services.

Hello-world example: <https://github.com/hyperium/tonic/blob/master/examples/helloworld/server.rs>

### grpc-rs (from TiKV)

<https://github.com/tikv/grpc-rs>

A Rust wrapper of the C++ gRPC Core. Used inside TiKV / TiDB; mature but C++ build dep.

### Native macOS client

<https://gripgrpc.dev/> — Grip, a native macOS gRPC client (think Postman for gRPC).

### Tutorials and reading

- [Quick start to gRPC using Rust (Geek Culture)](https://medium.com/geekculture/quick-start-to-grpc-using-rust-c655785fc6f4) — concise, recommended.
- [Tour of API protocols: gRPC + Apache Arrow Flight (DEV)](https://dev.to/alexmercedcoder/understanding-rpc-tour-of-api-protocols-grpc-nodejs-walkthrough-and-apache-arrow-flight-55bd)
- Official: <https://grpc.io/docs/languages/>

### Adjacent: distributed consensus

`raft-rs` — TiKV's Raft implementation. Not gRPC itself but lives in the same systems-distributed-coordination space and is often deployed *over* gRPC.

<https://github.com/tikv/raft-rs>

> When building a distributed system one principal goal is often to build in fault-tolerance — if a node goes down the cluster keeps working. Raft (and Paxos) is the consensus algorithm that makes that work; Raft-rs is used in TiKV and (in spirit) inspired etcd's implementation. Raft is generally considered easier to understand than Paxos.
