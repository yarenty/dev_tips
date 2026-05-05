---
title: Rust - Java interactions
main_link: https://github.com/giovanniberti/robusta
keywords: [to-java, interop, rust, programming, java, jni, type, questdb]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Rust - Java interactions

**Main link:** <https://github.com/giovanniberti/robusta>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[ballista_py]] — ballista py _(score 25.5)_
- [[python]] — PUFF _(score 25.5)_
- [[pyo3]] — pyo3 _(score 25.5)_
- [[questdb]] — QuestDB _(score 20.9)_
- [[rtic]] — RTIC _(score 14.7)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#to-java` `#interop` `#rust` `#programming` `#java` `#jni` `#type` `#questdb`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Rust - Java interactions

Interesting new way of accessing FFI in Java
https://openjdk.org/jeps/389
- mentions of JNI/ JNA / JNR / JavaCPP .. cpython

https://vived.substack.com/p/rust-written-jvm-and-bytecode-transpiler


## JNI


## JNIX

https://docs.rs/jnix/latest/jnix/


This crate provides high-level extensions to help with the usage of JNI in Rust code. Internally, it uses the jni-rs crate for the low-level JNI operations.

Some helper traits are provided, such as:

AsJValue: for allowing a JNI type to be convected to a JValue wrapper type.
IntoJava: for allowing a Rust type to be converted to a Java type.
FromJava: for allowing a Rust type to be created from a Java type.
A JnixEnv helper type is also provided, which is a JNIEnv wrapper that contains an internal class cache for preloaded classes.

If compiled with the derive feature flag, the crate also exports procedural macros to derive IntoJava and to derive FromJava, which makes writing conversion code a lot easier. An example would be:

```rust
use jnix::{
jni::{objects::JObject, JNIEnv},
JnixEnv, FromJava, IntoJava,
};

// Rust type definition
#[derive(Default, FromJava, IntoJava)]
#[jnix(package = "my.package")]
pub struct MyData {
number: i32,
string: String,
}

// A JNI function called from Java that creates a `MyData` Rust type, converts it to a Java
// type and returns it.
#[no_mangle]
#[allow(non_snake_case)]
pub extern "system" fn Java_my_package_JniClass_getData<'env>(
env: JNIEnv<'env>,
_this: JObject<'env>,
data: JObject<'env>,
) -> JObject<'env> {
// Create the `JnixEnv` wrapper
let env = JnixEnv::from(env);

    // Convert parameter to Rust type
    let data = MyData::from_java(&env, data);

    // Create a new `MyData` object by converting from the Rust type. Since a smart pointer is
    // returned from `into_java`, the inner object must be "leaked" sothat the garbage collector
    // can own it afterwards
    data.into_java(&env).forget()
}
```

```java
package my.package;

public class MyData {
public MyData(int number, String string) {
// This is the constructor that is called by the generated `IntoJava` code
//
// Note that the fields don't actually have to exist, the only thing that's necessary
// is for the target Java class to have a constructor with the expected type signature
// following the field order of the Rust type.
}

    // These getters are called by the generated `FromJava` code
    public int getNumber() {
        return 10;
    }

    public String getString() {
        return "string value";
    }
}
```


## QuestDB

There are some extensions on java side to use rust code

https://github.com/questdb/questdb/blob/master/core/rust/qdb-sqllogictest/src/sqllogictest/mod.rs

lookig for the fact that it has some structure pattern this could be good point to write pyo3 style library for Java

Also initial article:

https://questdb.io/blog/leveraging-rust-in-our-high-performance-java-database/








## Robusta

7 yo :-(
https://github.com/giovanniberti/robusta

article:
https://medium.com/@shmulikamar/https-medium-com-shmulikamar-connecting-python-and-java-with-rust-11c256a1dfb0


## jtcpp

java to cpp in rust
https://github.com/FractalFir/jtcpp
