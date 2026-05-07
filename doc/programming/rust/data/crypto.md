---
title: RustCrypto — POLYVAL (and the wider ecosystem)
main_link: https://github.com/RustCrypto
keywords: [rustcrypto, polyval, aes, gcm, gcm-siv, mac]
status: reviewed
---

# RustCrypto — POLYVAL (and the wider ecosystem)

**Main link:** <https://github.com/RustCrypto>

## Summary

The RustCrypto org maintains a large set of pure-Rust cryptography crates: hashes (`sha2`, `blake3`, `sha3`), AEADs (`aes-gcm`, `aes-gcm-siv`, `chacha20poly1305`), MACs (`hmac`, `polyval`, `ghash`), block / stream ciphers, KDFs, and signatures (`ed25519-dalek`, `ecdsa`, `rsa`). This article in particular covers `polyval` — a universal hash function over GF(2¹²⁸) defined by RFC 8452, used inside AES-GCM-SIV and (via its close relative GHASH) inside AES-GCM. You almost never call `polyval` directly; you reach for `aes-gcm-siv` or `aes-gcm` and they pull in `polyval`/`ghash` underneath.

## Insight

The Rust crypto landscape today has three layers: (1) **algorithms** — the RustCrypto org's per-algorithm crates, lots of small focused dependencies; (2) **TLS / protocol stacks** — `rustls` (the de-facto Rust TLS, increasingly replacing OpenSSL/native-tls), `quinn` (QUIC), `snow` (Noise); (3) **wrapping libraries** — `ring` (a curated subset, used by `rustls`), `dalek-cryptography` (Curve25519 / Ristretto / Bulletproofs), `sodiumoxide` (libsodium FFI, **deprecated** — the project recommends migrating to `libsodium-rs` or RustCrypto). For most apps the right reach is (a) `rustls` for TLS, (b) `aes-gcm` or `chacha20poly1305` from RustCrypto for at-rest encryption, (c) `ed25519-dalek` for signatures, (d) `argon2` for password hashing. Avoid rolling your own. Reach for `polyval` directly only if you're implementing AES-GCM-SIV-adjacent protocols by hand. Gotcha: `aes-gcm-siv` is *misuse-resistant* (nonce reuse leaks only equality, not the key) — prefer it over `aes-gcm` when you can't guarantee unique nonces.

## Similar / related topics

- `aes-gcm` / `aes-gcm-siv` / `chacha20poly1305` — AEADs that use polyval under the hood.
- `rustls` — pure-Rust TLS stack; the modern default vs OpenSSL.
- `ring` — curated subset of BoringSSL bindings; `rustls` uses it.
- `ed25519-dalek` / `x25519-dalek` — Curve25519 signatures and DH.
- `argon2` / `bcrypt` — password hashing (for storage, not auth tokens).
- `sodiumoxide` — libsodium FFI (deprecated as of 2023).

## Internal links
- [[../net/README|Rust net]] — TLS/QUIC live there.
- [[../core/README|Rust core]]

## Keywords

`#rustcrypto` `#polyval` `#aes-gcm` `#aes-gcm-siv` `#mac`

## References / raw notes

- RustCrypto org: <https://github.com/RustCrypto>
- POLYVAL crate: <https://crates.io/crates/polyval>
- AEADs collection: <https://github.com/RustCrypto/AEADs>
- Hashes collection: <https://github.com/RustCrypto/hashes>
- MACs collection: <https://github.com/RustCrypto/MACs>

> POLYVAL (RFC 8452) is a universal hash function operating over GF(2¹²⁸), used to construct a Message Authentication Code (MAC).
>
> Primary intended use: implementing AES-GCM-SIV. Closely related to GHASH; can also implement AES-GCM at no performance cost on little-endian architectures.
