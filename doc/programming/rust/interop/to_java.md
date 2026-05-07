---
title: Rust ↔ JVM interop
main_link: https://github.com/jni-rs/jni-rs
keywords: [rust, java, jvm, jni, robusta, jnix, j4rs, duchess, ffm, questdb]
status: reviewed
---

# Rust ↔ JVM interop

**Main link:** <https://github.com/jni-rs/jni-rs>

## Summary

The canonical Rust↔Java bridge is the **JNI** — Java's
30-year-old C-ABI escape hatch — wrapped on the Rust side by the
[`jni`](https://github.com/jni-rs/jni-rs) crate and a small ecosystem of
ergonomic layers on top of it (`jnix`, `robusta`, `j4rs`, `duchess`).
Modern alternatives are emerging: Java 22's stable
[Foreign Function & Memory API (JEP 454)](https://openjdk.org/jeps/454)
deprecates JNI for new code in favour of `Linker`/`MemorySegment`,
which can call any Rust `cdylib` with no JNI wrapper at all.
[QuestDB](https://questdb.io/blog/leveraging-rust-in-our-high-performance-java-database/)
is the leading real-world example of a high-performance JVM database
calling into Rust for hot paths.

## Insight

Pick by direction and ergonomic appetite:

- **Java calls into Rust (hot path / native lib)** — write a Rust
  `cdylib` with `extern "system" fn Java_pkg_Class_method(...)`
  signatures. Use the `jni` crate raw, or `jnix` for `IntoJava`/`FromJava`
  derives, or `robusta` for `#[derive(Signature)]`-style sugar. For
  Java 22+ you can skip JNI entirely and use the
  [Foreign Function & Memory API (FFM)](https://openjdk.org/jeps/454)
  with `jextract` to call your `cdylib` directly.
- **Rust calls into Java (drive a JVM from Rust)** — `j4rs` is the
  go-to: it boots a JVM, lets you call arbitrary Java classes, and
  marshals types via JSON. Useful when you must reuse a large Java
  library from a Rust process.
- **Generated bindings** — `duchess` (Niko Matsakis et al.) reads Java
  classes and emits typed Rust wrappers; less mature but the future
  direction.
- **Cross-compile to JVM bytecode** — `jtcpp` and similar experiments
  attempt Rust→JVM/CLR codegen; nowhere near production. The .NET
  cousin is `rustc_codegen_clr` (see [[to_net]]).

Ecosystem realities to budget for:

- **JNI overhead per call** is non-trivial (several hundred ns each
  way); design batched APIs, not chatty per-row callbacks. QuestDB's
  blog post is the canonical war story.
- **Class-loader / GC interactions**: JNI local refs leak if you don't
  drop them; `jnix::JnixEnv` has a class cache for hot lookups.
- **`#[no_mangle] pub extern "system"`** is mandatory; the function name
  must match `Java_<package>_<Class>_<method>` exactly (with `_` → `_1`
  escapes for underscores in package names — read the JNI spec).
- **Apache Arrow Java↔Rust** ships a separate
  [Arrow Java JNI module](https://arrow.apache.org/docs/java/jni.html)
  for sharing record batches across the boundary without copy.
- **`robusta` is unmaintained** (last commit 2022); fine for learning,
  not for new prod code. Prefer `jni` raw + manual macros, or wait on
  `duchess`.

## Similar / related topics

- [[pyo3|PyO3]] — the equivalent for Python; same FFI shape, much more
  polished.
- [[to_net]] — the .NET equivalent (Rust↔CLR).
- Java 22 FFM API (JEP 454) — modern post-JNI alternative.
- Apache Arrow Java JNI — zero-copy record-batch interop.
- GraalVM native-image — opposite direction (compile JVM to native; not
  Rust-specific but eliminates the FFI question).

## Internal links

- [[pyo3|PyO3]] — Python equivalent.
- [[to_net]] — .NET equivalent.
- [[README]] — Rust interop landing.

## Keywords

`#rust` `#java` `#jvm` `#jni` `#ffm` `#interop` `#questdb`

## References / raw notes

- [`jni` crate (jni-rs/jni-rs)](https://github.com/jni-rs/jni-rs) —
  canonical low-level Rust JNI binding.
- [JEP 454 — Foreign Function & Memory API](https://openjdk.org/jeps/454)
  — replaces JNI for most new code.
- ["Rust-written JVM and bytecode transpiler" (vived.substack.com)](https://vived.substack.com/p/rust-written-jvm-and-bytecode-transpiler)

### `jnix` (high-level wrapper)

[`jnix`](https://docs.rs/jnix/latest/jnix/) provides high-level extensions
on top of `jni`. It exposes traits like `AsJValue`, `IntoJava`,
`FromJava`, plus a `JnixEnv` JNIEnv wrapper with an internal class cache
for preloaded classes. With the `derive` feature, procedural macros let
you write:

```rust
use jnix::{
    jni::{objects::JObject, JNIEnv},
    JnixEnv, FromJava, IntoJava,
};

#[derive(Default, FromJava, IntoJava)]
#[jnix(package = "my.package")]
pub struct MyData {
    number: i32,
    string: String,
}

#[no_mangle]
#[allow(non_snake_case)]
pub extern "system" fn Java_my_package_JniClass_getData<'env>(
    env: JNIEnv<'env>,
    _this: JObject<'env>,
    data: JObject<'env>,
) -> JObject<'env> {
    let env = JnixEnv::from(env);
    let data = MyData::from_java(&env, data);
    data.into_java(&env).forget()
}
```

```java
package my.package;

public class MyData {
    public MyData(int number, String string) { /* ctor matched by IntoJava */ }
    public int getNumber() { return 10; }
    public String getString() { return "string value"; }
}
```

### QuestDB

[QuestDB](https://github.com/questdb/questdb) — JVM time-series database
that selectively delegates hot loops to Rust. Worth reading as a
production case study; e.g.
[`qdb-sqllogictest`](https://github.com/questdb/questdb/blob/master/core/rust/qdb-sqllogictest/src/sqllogictest/mod.rs).
Their blog post
["Leveraging Rust in our high-performance Java database"](https://questdb.io/blog/leveraging-rust-in-our-high-performance-java-database/)
covers the JNI design choices, batching, and benchmark wins.

### Robusta

[`robusta`](https://github.com/giovanniberti/robusta) — `#[derive]`-driven
JNI wrapper. Note the project is largely dormant (last meaningful activity
2022); prefer `jni` raw or `duchess` for new work. Companion blog:
["Connecting Python and Java with Rust"](https://medium.com/@shmulikamar/https-medium-com-shmulikamar-connecting-python-and-java-with-rust-11c256a1dfb0).

### `j4rs`

[`j4rs`](https://github.com/astonbitecode/j4rs) — Rust-drives-Java: boot a
JVM from a Rust process and call arbitrary Java classes. The inverse of
the JNI flow above.

### `duchess`

[`duchess`](https://github.com/duchess-rs/duchess) — Niko Matsakis et al.;
generates typed Rust wrappers from Java class metadata. Most ergonomic of
the modern crop; still pre-1.0.

### `jtcpp`

[`jtcpp`](https://github.com/FractalFir/jtcpp) — experimental Java→C++
transpiler in Rust by FractalFir (also the author of
`rustc_codegen_clr`). Research-grade, included for completeness.
