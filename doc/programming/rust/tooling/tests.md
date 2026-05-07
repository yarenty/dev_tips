---
title: tests — `mockall` and the Rust testing-tools landscape
main_link: https://crates.io/crates/mockall
keywords: [tests, mockall, rust, testing, mocks, nextest, criterion, proptest]
status: reviewed
---

# tests — `mockall` and the Rust testing-tools landscape

**Main link:** <https://crates.io/crates/mockall>

## Summary

`mockall` is the canonical Rust mock-object library by Alan Somers — a `#[automock]` proc-macro that derives a `MockMyTrait` for any trait, plus a builder API for setting expectations. Mocking statically-typed Rust is harder than mocking duck-typed Python, and `mockall` is the most ergonomic answer in the ecosystem. This article also serves as the **Rust testing-tools landscape** index, since the auto-split previously left this as the parent of [[tux]] and [[turmoil]].

## Insight

Reach for `mockall` when you need to **inject a mock implementation of a trait** in a unit test — typically to replace a database, HTTP client, or message bus with a scripted set of responses. Standard pattern:

```rust
use mockall::*;

#[automock]
trait Database {
    fn get_user(&self, id: u64) -> Option<String>;
}

#[test]
fn user_service_returns_name() {
    let mut db = MockDatabase::new();
    db.expect_get_user()
        .with(predicate::eq(42))
        .times(1)
        .returning(|_| Some("Alice".into()));

    let svc = UserService::new(Box::new(db));
    assert_eq!(svc.greet(42), "Hello, Alice");
}
```

**Gotchas**: requires `dev-dependencies = { mockall = "..." }` and you typically gate the `automock` attribute behind `#[cfg(test)]` to avoid leaking the mock type into production builds; trait must be object-safe if you'll use `Box<dyn Trait>`; closures-as-return-values can be tricky to mock. For *external HTTP* mocking specifically, prefer `[wiremock](https://crates.io/crates/wiremock)` over rolling your own `MockHttpClient`.

### The broader Rust testing landscape

| Need | Reach for |
|---|---|
| **Test runner** with parallelism / retries / per-test timeout | [`cargo-nextest`](https://nexte.st/) — modern replacement for the built-in test runner |
| **Mock objects** for traits | `mockall` (this article) |
| **HTTP mocking / fakes** | [`wiremock`](https://crates.io/crates/wiremock), [`mockito`](https://crates.io/crates/mockito), [`httpmock`](https://crates.io/crates/httpmock) |
| **Property-based testing** | [`proptest`](https://crates.io/crates/proptest), [`quickcheck`](https://crates.io/crates/quickcheck) |
| **Fuzzing** | `cargo-fuzz` (libFuzzer), [`afl.rs`](https://github.com/rust-fuzz/afl.rs) |
| **Snapshot testing** | [`insta`](https://crates.io/crates/insta) — the canonical snapshot crate |
| **Microbenchmarks** | [`criterion`](https://crates.io/crates/criterion) — the standard; `divan` is the modern challenger |
| **Coverage** | [`cargo-tarpaulin`](https://github.com/xd009642/tarpaulin) (Linux), `cargo-llvm-cov` (cross-platform) |
| **Integration tests against HTTP / temp dirs / binaries** | [[tux]] |
| **Distributed-system tests** | [[turmoil]] (Tokio team's deterministic simulator) |
| **Concurrency model checking** | `loom` — explores all interleavings of an async/atomics test |
| **Async coloring** in tests | `tokio::test`, `async-std::test` |

## Similar / related topics

- [[tux]] — integration-test helpers (HTTP, temp dirs, binary execution).
- [[turmoil]] — deterministic distributed-system simulation.
- [`cargo-nextest`](https://nexte.st/) — better test runner.
- [`insta`](https://crates.io/crates/insta) — snapshot testing.
- [`criterion`](https://crates.io/crates/criterion) — microbenchmarks.
- [`proptest`](https://crates.io/crates/proptest) / [`quickcheck`](https://crates.io/crates/quickcheck) — property-based.
- [`wiremock`](https://crates.io/crates/wiremock) — HTTP mocking.

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[tux]] — sibling integration-test helpers.
- [[turmoil]] — deterministic distributed-system tests.
- [[../core/error|core/error]] — error-handling overview that pairs with test design.

## Keywords

`#tests` `#mockall` `#rust` `#testing` `#mocks` `#nextest` `#criterion` `#proptest`

## References / raw notes

- Crate: <https://crates.io/crates/mockall>
- Docs: <https://docs.rs/mockall/>
- Mockall is 100% safe stable Rust; supports static and `dyn` traits; multi-thread-safe expectations.
