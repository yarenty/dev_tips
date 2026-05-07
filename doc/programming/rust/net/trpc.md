---
title: "tRPC — end-to-end typesafe APIs (TypeScript) and the Rust angle"
main_link: https://trpc.io/
keywords: [trpc, typescript, rpc, end-to-end-types, fullstack, react, rust]
status: reviewed
---

# tRPC — end-to-end typesafe APIs (TypeScript) and the Rust angle

**Main link:** <https://trpc.io/>

## Summary

[**tRPC**](https://trpc.io/) is a **TypeScript-only** RPC framework that lets a TS client import the *type signature* of the server's procedures directly — no codegen, no schema files, no protobuf. Calling `trpc.user.byId.query(123)` on the client is end-to-end type-checked against the `userRouter` on the server. It's filed in this `programming/rust/net/` section as a **comparison point** for Rust API designers — there is no first-party Rust port, and "tRPC for Rust" is best read as "Rust's typed-RPC alternatives" (gRPC + tonic, tarpc, RSPC).

## Insight

tRPC's whole pitch is **single-language full-stack** — a Next.js / SvelteKit / Solid / React-Native app where the same TypeScript repo holds both server and client. The type-import-across-the-network trick is impossible in heterogeneous (Rust-server / TS-client) setups, which is the exact reason gRPC + codegen exists.

Where tRPC fits and where the Rust analogues fit:

| Stack | Pick |
|-------|------|
| TS server + TS client (Next.js, etc.) | **tRPC** — no contest |
| Rust server + TS client | gRPC + [`tonic`](grpc.md) + protoc-gen-ts, or [Connect](https://connectrpc.com/) |
| Rust server + Rust client | [`tarpc`](https://github.com/google/tarpc) (Google) — type-shared via a common crate, the closest analogue to tRPC's ergonomic |
| Rust server + TS client, hate codegen | [**RSPC**](https://www.rspc.dev/) — explicit "tRPC for Rust"; type bridge to TS via `specta` |

The "tRPC for Rust" pattern in practice:

- Define your procedures in a shared Rust crate.
- Use [`tarpc`](https://github.com/google/tarpc) for Rust↔Rust, sharing the trait crate.
- Use [`rspc`](https://www.rspc.dev/) for Rust↔TS, exporting types to `.ts` at build time via `specta`.
- For external APIs, just use gRPC (`tonic`) or REST + OpenAPI (`utoipa` / `aide`) — that's what the rest of the world expects.

Things tRPC gets right that are easy to miss:

1. **No codegen step** in the build pipeline. The TS compiler does the work; CI is simpler.
2. **No runtime overhead** beyond JSON. Wire format is just HTTP + JSON.
3. **First-class React Query integration** (`@trpc/react-query`) — caching / invalidation / optimistic updates for free.
4. **Adapter pattern** for Express, Fastify, Next.js, Cloudflare Workers, AWS Lambda — easy to add to brownfield TS apps.

Tradeoffs that come up:

1. **Wire is JSON.** No binary, no schema-on-the-wire. Bigger payloads than protobuf.
2. **No interop story.** A non-TS client (mobile native, Rust, Go) cannot consume a tRPC API directly without re-implementing a JSON shape per route.
3. **Type-checking isn't runtime validation.** You still need [zod](https://zod.dev/) (which tRPC integrates with) on the server boundary; types alone don't catch malformed input from the wire.

## Similar / related topics

- [`tonic`](grpc.md) — the gRPC story for Rust; the right answer when you need cross-language interop.
- [`tarpc`](https://github.com/google/tarpc) — Google's Rust-only typed RPC; closest to tRPC's "share the type, no codegen" feel for all-Rust stacks.
- [RSPC](https://www.rspc.dev/) — explicit "tRPC for Rust" project; Rust server + auto-generated TS client types.
- [Connect](https://connectrpc.com/) — gRPC-compatible protocol that also works HTTP/1.1 + JSON; a sweet spot when you want browser-friendly + schema-driven.
- [zod](https://zod.dev/) — runtime validation in TS; tRPC pairs with it for input parsing.
- [oRPC](https://orpc.unnoq.com/) — newer TS RPC framework with OpenAPI compatibility built in.

## Internal links

<!-- reviewed -->

- [[programming/rust/net/README|Rust net]] — section landing page.
- [[grpc|tonic / gRPC]] — the cross-language sibling.
- [[programming/rust/web/README|Rust web]] — sibling section for HTTP / REST APIs.

## Keywords

`#trpc` `#typescript` `#rpc` `#end-to-end-types` `#fullstack` `#rust-comparison`

## References / raw notes

### tRPC

<https://trpc.io/>

> End-to-end typesafe APIs made easy.

- **Automatic typesafety** — types of API paths, inputs, and outputs are inferred from the server and shared with the client at compile time. No codegen.
- **Light & snappy DX** — no codegen, no run-time bloat, no build pipeline, zero deps, tiny client footprint.
- **Add to brownfield** — adapters for Connect/Express, Fastify, Next.js, Cloudflare Workers, AWS Lambda.

`@trpc/client` depends on Babel runtime helpers and uses a `fetch()` polyfill in browsers that need one. `@trpc/react` is built on top of TanStack Query (formerly `react-query`).

### Rust-side analogues

- [`tarpc`](https://github.com/google/tarpc) — Google's Rust RPC; share a trait crate for type safety.
- [RSPC](https://www.rspc.dev/) — "tRPC for Rust" project, exports TS bindings via `specta`.
- [`tonic`](https://github.com/hyperium/tonic) — gRPC in Rust; the cross-language standard.
