---
title: "Java"
keywords: [java, jvm, kotlin, graalvm, jdk, virtual-threads, native-image]
status: reviewed
review_date: 2026/05/03
---

# Java

Sparse landing — no in-depth Java articles here yet. Java itself is in a much better place than its reputation suggests; if you last touched it in the Java 8 era you'd be surprised.

## Modern Java in one paragraph

Since the move to a six-month release cadence (Java 9+), the JVM has accumulated a steady stream of genuinely useful additions: records, sealed classes, pattern matching for `switch`, text blocks, `var`, and — most consequentially — **virtual threads** (Project Loom, GA in JDK 21). Virtual threads make blocking I/O cheap again and let you write straightforward synchronous code without paying the throughput price you used to pay. JDK 21 is the current LTS most projects target; JDK 25 is the next.

## What's worth knowing about

- **JDK distributions** — Oracle JDK is no longer the default choice. Use [Adoptium Temurin](https://adoptium.net/), Amazon Corretto, Azul Zulu, or Microsoft Build of OpenJDK. All are OpenJDK builds.
- **Kotlin** — JetBrains' alternative JVM language. Pragmatically nicer than Java for most tasks; the standard choice for Android. Interops both ways with Java seamlessly.
- **Scala** — see [[programming/scala/README|Scala]]; very different philosophy.
- **GraalVM Native Image** — AOT-compiles a JVM application to a native binary with sub-100ms startup. Game-changer for CLIs and serverless. Cost: reflection / dynamic class loading needs configuration.
- **Build tools** — [Gradle](https://gradle.org/) (Kotlin DSL recommended over Groovy), [Maven](https://maven.apache.org/), [Bazel](https://bazel.build/) at scale, [Mill](https://mill-build.org/) for the brave.
- **Frameworks** — Spring Boot still dominates; [Quarkus](https://quarkus.io/) and [Micronaut](https://micronaut.io/) are the GraalVM-native-friendly alternatives; [Helidon](https://helidon.io/) is Oracle's take.

## Canonical external resources

- [dev.java](https://dev.java/) — official learning portal.
- [JEPs (Java Enhancement Proposals)](https://openjdk.org/jeps/0) — what's actually shipping in the language.
- [Inside Java](https://inside.java/) — the official OpenJDK team blog/podcast.
- [Baeldung](https://www.baeldung.com/) — the most-linked tutorial site for "how do I do X in Spring".
- [*Effective Java* (Bloch)](https://www.oreilly.com/library/view/effective-java-3rd/9780134686097/) — still the canonical idiom book.

## See also

- [[programming/scala/README|Scala]] — sibling JVM language.
- [[programming/package_managers/README|package managers]] — Maven / Gradle context.
- [[programming/README|programming]] — parent.

## Keywords

`#java` `#jvm` `#kotlin` `#graalvm` `#jdk` `#virtual-threads` `#native-image` `#programming`
