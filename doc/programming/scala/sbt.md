---
title: "sbt — the Scala Build Tool"
main_link: https://www.scala-sbt.org/
keywords: [sbt, scala, build-tool, jvm, mill, scala-cli, incremental-compilation]
status: reviewed
---

# sbt — the Scala Build Tool

**Main link:** <https://www.scala-sbt.org/>

Docs: <https://www.scala-sbt.org/1.x/docs/> · Repo: <https://github.com/sbt/sbt>

## Summary

sbt is the de-facto build tool for Scala. It does what Maven and Gradle do for the wider JVM, but with a Scala-native DSL, first-class support for cross-compiling against multiple Scala versions, and tight integration with the incremental Scala compiler (Zinc). Its trademark is the interactive shell — once a project is loaded, commands like `~compile`, `~test`, and `~run` watch the filesystem and re-run on change, which makes the inner loop pleasant once you've paid the startup cost.

## Insight

The honest reputation: sbt is **powerful but slow to start, hard to learn, and famously easy to misuse**. The DSL is a Scala-internal one, so a `build.sbt` looks deceptively simple until you need to do something custom and find yourself reading `sbt-plugin` source code. The startup cost of "open shell, load build, resolve dependencies" can dominate quick edit-compile-test loops if you exit the shell each time.

Strategies that make sbt much more bearable:

- **Stay in the shell.** Start `sbt` once and leave it running. Use `~compile` / `~test` for watch mode. Don't invoke `sbt foo` from the command line repeatedly — that pays the startup cost every time.
- **Use [`sbt-bloop`](https://scalacenter.github.io/bloop/) or Bloop directly.** Bloop is a build server that takes sbt's project model and serves it to IDEs and CLIs much faster than sbt itself.
- **Project structure**: keep the sub-project graph reasonable. sbt's incrementality breaks down on very large projects with many sub-projects.
- **Scala Steward** (<https://github.com/scala-steward-org/scala-steward>) — automated dependency PRs; effectively required for any non-trivial project.

The realistic alternatives in 2024-25:

- **[Mill](https://mill-build.org/)** — Li Haoyi's build tool. Faster, much simpler mental model (build files are just Scala objects with methods), gaining real adoption. The strongest "would I start a new project in sbt?" challenger.
- **[scala-cli](https://scala-cli.virtuslab.org/)** — for scripts, single-file projects, examples, learning. Massively friendlier than sbt for "I just want to try this snippet."
- **[Bazel](https://bazel.build/) / [Pants](https://www.pantsbuild.org/)** — at large polyglot monorepo scale.
- **Maven / Gradle** — possible but rarely used for Scala anymore; the Scala-version-cross story is awkward.

When sbt is still the right answer:

- Existing Scala project — sbt is the path of least resistance.
- You depend on a plugin ecosystem (sbt-native-packager, sbt-assembly, sbt-mdoc, sbt-pgp, ...) that's most mature on sbt.
- You need cross-compilation against multiple Scala versions and multiple platforms (JVM / Native / JS) and the corresponding cross-projects (`++3.3.x`, `+publishSigned`, etc.) plugin matters.

## Configuration tips

- `$SBT_OPTS` passes additional JVM options to sbt globally.
- Per-project options go in `.sbtopts` at the repo root.
- Global system-wide settings go in `/usr/local/etc/sbtopts` (Homebrew install) or the equivalent per platform.
- For memory-hungry projects, bump `-Xmx` here rather than in `JAVA_OPTS` (which sbt overrides).

## Similar / related topics

- [Mill](https://mill-build.org/) — the leading sbt alternative for new projects.
- [scala-cli](https://scala-cli.virtuslab.org/) — for scripts and quick experiments.
- [Bloop](https://scalacenter.github.io/bloop/) — build server that accelerates sbt-defined projects.
- [Coursier](https://get-coursier.io/) — the underlying dependency resolver sbt uses.
- [Gradle](https://gradle.org/) — closest spiritual sibling on the broader JVM side.

## Internal links

<!-- reviewed -->

- [[programming/scala/README|Scala]] — parent section.
- [[programming/java/README|Java]] — JVM context.
- [[programming/package_managers/README|package managers]] — sbt's resolver pulls from Maven Central.

## Keywords

`#sbt` `#scala` `#build-tool` `#jvm` `#mill` `#scala-cli` `#incremental-compilation` `#programming`

## References / raw notes

From the project shell help:

> You can use `$SBT_OPTS` to pass additional JVM options to sbt.
> Project-specific options should be placed in `.sbtopts` in the root of your project.
> Global settings should be placed in `/usr/local/etc/sbtopts`.
