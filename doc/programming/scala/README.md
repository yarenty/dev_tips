---
title: "Scala"
keywords: [scala, scala-3, akka, pekko, zio, cats-effect, scala-native, scala-js]
status: reviewed
---

# Scala

A small landing for Scala. The articles here are limited; the language itself is much larger than this section reflects.

## Where Scala stands today

- **Scala 3 vs Scala 2.13**: Scala 3 (Dotty) is the future and most new code should target it. The migration is mostly mechanical for application code; library authors have a harder time supporting both. Scala 2.13 still has a long maintenance tail, especially in enterprise.
- **The Akka split**: Lightbend re-licensed Akka under the Business Source License in September 2022. The Apache Foundation forked the last Apache-2.0 release as **[Apache Pekko](https://pekko.apache.org/)**. For new open-source work, prefer Pekko unless you're already a Lightbend customer.
- **Effect systems**: [ZIO](https://zio.dev/) and [Cats Effect](https://typelevel.org/cats-effect/) are the two dominant pure-FP runtimes. Pick one and commit — they do not mix gracefully. ZIO has a more approachable on-ramp; Cats Effect is more aligned with Haskell-flavored type-class FP.
- **Compilation targets**: [Scala Native](https://scala-native.org/) (LLVM, native binaries) and [Scala.js](https://www.scala-js.org/) (JS / browser) are real and used in production, though the JVM remains the primary target.
- **scala-cli**: [scala-cli](https://scala-cli.virtuslab.org/) is the modern "just run this Scala file" tool. For scripts and small projects it's much friendlier than sbt.

## Articles

- [[sbt]] — Scala Build Tool: the historic default; powerful, slow, polarizing.

## Canonical external resources

- [Scala 3 Book](https://docs.scala-lang.org/scala3/book/introduction.html) — the official intro.
- [scala-lang.org](https://www.scala-lang.org/) — homepage with releases and docs.
- [Rock the JVM](https://rockthejvm.com/) — the most-recommended modern Scala course material.
- [*Functional Programming in Scala* (the "red book")](https://www.manning.com/books/functional-programming-in-scala-second-edition) — canonical FP intro using Scala.

## Build tools beyond sbt

- [Mill](https://mill-build.org/) — the most credible sbt alternative; faster, simpler, gaining traction.
- [scala-cli](https://scala-cli.virtuslab.org/) — for scripts, small projects, learning.
- [Bazel](https://bazel.build/) / [Pants](https://www.pantsbuild.org/) — at large monorepo scale.

## See also

- [[programming/java/README|Java]] — sibling JVM language.
- [[programming/package_managers/README|package managers]] — Maven Central context.
- [[programming/README|programming]] — parent.

## Keywords

`#scala` `#scala-3` `#akka` `#pekko` `#zio` `#cats-effect` `#scala-native` `#scala-js` `#programming`
