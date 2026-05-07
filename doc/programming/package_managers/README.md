---
title: "Package managers"
keywords: [package-managers, npm, cargo, pip, uv, go-mod, maven, gradle, composer, bun, pnpm, lockfiles]
status: reviewed
review_date: 2026/05/03
---

# Package managers

Sparse cross-language landing — no in-depth articles yet, but enough patterns recur across ecosystems that it's worth collecting them in one place.

## The cross-language landscape

| Language | Default | Lockfile | Notable alternatives |
|---|---|---|---|
| JavaScript / Node | npm | `package-lock.json` | [pnpm](https://pnpm.io/) (content-addressed, fast), [Yarn](https://yarnpkg.com/), [Bun](https://bun.sh/) |
| Rust | cargo | `Cargo.lock` | (cargo is universal; `sccache` for caching) |
| Python | pip | — (historically) | [uv](https://docs.astral.sh/uv/) (the modern winner), [Poetry](https://python-poetry.org/), [pdm](https://pdm-project.org/), [Pixi](https://pixi.sh/), conda/mamba |
| Go | `go mod` | `go.sum` | (one ecosystem) |
| JVM | Maven / Gradle | `gradle.lockfile` (opt-in) | [sbt](https://www.scala-sbt.org/) for Scala, [Mill](https://mill-build.org/) |
| Ruby | gem + Bundler | `Gemfile.lock` | — |
| PHP | [Composer](https://getcomposer.org/) | `composer.lock` | — |
| Swift | Swift Package Manager | `Package.resolved` | CocoaPods (legacy), Carthage (legacy) |
| Haskell | cabal / stack | `cabal.project.freeze` / `stack.yaml.lock` | — |
| OCaml | [opam](https://opam.ocaml.org/) | lock files via `opam lock` | — |
| Erlang / Elixir | rebar3 / mix | `mix.lock` | — |
| C/C++ | (none stdlib) | — | [Conan](https://conan.io/), [vcpkg](https://vcpkg.io/) |

## Recurring themes worth understanding once

- **Lockfiles** vs. **manifests**. Manifests (`package.json`, `Cargo.toml`, `pyproject.toml`) declare *what* you depend on, often with version ranges. Lockfiles (`package-lock.json`, `Cargo.lock`, `uv.lock`) record the *exact* resolved versions. Commit both for applications; commit only the manifest for libraries (with one big asterisk: cargo recommends committing `Cargo.lock` even for libs in some cases).
- **Transitive dependency hell**. The diamond problem (A depends on B and C, both of which depend on different versions of D) is solved differently per ecosystem: npm allows multiple versions side-by-side; Maven picks "nearest wins"; cargo enforces semver compatibility within major versions.
- **Security audits**. `npm audit`, `cargo audit`, `pip-audit`, `bundler-audit`, `composer audit` — all use the [OSV](https://osv.dev/) database or ecosystem-specific advisory feeds. Run them in CI.
- **Mirror / proxy caches**. [Verdaccio](https://verdaccio.org/) for npm, [`cargo-cache` + Cloudsmith](https://cloudsmith.com/) for cargo, [devpi](https://devpi.net/) for pip, [Artifactory](https://jfrog.com/artifactory/) / [Nexus](https://www.sonatype.com/products/sonatype-nexus-repository) for everything. Worth setting up for any team larger than one.
- **Supply-chain attacks**. The [event-stream](https://snyk.io/blog/malicious-code-found-in-npm-package-event-stream/), [colors.js / faker.js](https://snyk.io/blog/open-source-npm-packages-colors-faker/), and [PyPI typosquatting](https://www.theregister.com/2024/04/15/pypi_supply_chain_attacks/) incidents are required reading. Consider [Sigstore](https://www.sigstore.dev/) / signed releases / pinning to git revisions for high-stakes dependencies.
- **Bun, uv, and the "Rust rewrite" trend**. Several modern package managers (uv, Bun's installer, Astral's overall stack, Mago for PHP linting) are written in Rust for the obvious reason: dependency resolution and disk I/O are exactly the workload Rust excels at. Expect this trend to continue.

## See also

- [[programming/rust/README|Rust]] — cargo is the gold standard most others now copy.
- [[programming/php/README|PHP]] — composer notes there.
- [[programming/scala/README|Scala]] — sbt notes there.
- [[programming/cpp/README|C++]] — Conan / vcpkg context.
- [[programming/README|programming]] — parent.

## Keywords

`#package-managers` `#npm` `#cargo` `#pip` `#uv` `#go-mod` `#maven` `#composer` `#lockfiles` `#programming`
