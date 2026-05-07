---
title: matrix-rust-sdk (Apple bindings)
main_link: https://github.com/matrix-org/matrix-rust-sdk/tree/main/bindings/apple
keywords: [matrix, matrix-rust-sdk, ios, apple, bindings, chat-protocol, element]
status: reviewed
---

# matrix-rust-sdk (Apple bindings)

**Main link:** <https://github.com/matrix-org/matrix-rust-sdk/tree/main/bindings/apple>

## Summary

Reference to the **Apple bindings** subtree of `matrix-rust-sdk` — the Matrix protocol client SDK that Element iOS (and Element X iOS) use to talk to a Matrix homeserver (e.g. Synapse, Dendrite, Conduit). The Rust core handles state sync, end-to-end encryption (Olm/Megolm), key verification, and the room model; the `bindings/apple` directory exposes that core to Swift via Mozilla's `uniffi` so iOS / macOS apps can link a single shared `.xcframework`.

## Insight

This is filed under `gui/` because it's the closest the vault has to "real iOS app SDK", but the article is really a pointer: don't reinvent a Matrix client in Swift, link `matrix-rust-sdk` and write the UI on top. The same Rust core is the basis for clients on Android (via `bindings/jvm` + uniffi), for Element Web's experimental Rust crypto, and for `matrix-sdk-crypto` (the standalone crypto crate that `python-matrix-nio` and others depend on). Confusingly, "Matrix" in the Rust ecosystem can also mean (a) the linear-algebra type in `nalgebra` / `glam` (very common in graphics/ML code) or (b) Matrix.org the chat protocol — this article is the chat protocol. For a non-Matrix-protocol primer on the Rust SDK, see Element's blog and the [`docs.rs/matrix-sdk`](https://docs.rs/matrix-sdk) reference.

## Similar / related topics

- `matrix-sdk-crypto` — the standalone E2EE crate; can be embedded without the full client.
- `bindings/jvm` — the Android side; same Rust core, JNI front.
- Element X iOS — the production app shipping on top of these bindings.
- `simplex-chat` / `xmtp-rs` — alternative end-to-end encrypted protocols with Rust SDKs.
- `nalgebra` / `glam` — the *other* "matrix" in Rust (linear algebra; not this).

## Internal links

- [[macos]]
- [[mobile]]
- [[cacao]]
- [[../README]]

## Keywords

`#matrix` `#matrix-rust-sdk` `#ios` `#apple` `#bindings` `#chat-protocol` `#e2ee`

## References / raw notes

- Apple bindings: <https://github.com/matrix-org/matrix-rust-sdk/tree/main/bindings/apple>
- Top-level repo: <https://github.com/matrix-org/matrix-rust-sdk>
- Crate docs: <https://docs.rs/matrix-sdk>
- Matrix specification: <https://spec.matrix.org/>
