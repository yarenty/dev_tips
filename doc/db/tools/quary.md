---
title: Quary
main_link: https://github.com/quarylabs/quary
keywords: [quary, sql, dbt-alternative, transformation, duckdb, vscode, rust, models]
status: reviewed
review_date: 2026/05/03
---

# Quary

**Main link:** <https://github.com/quarylabs/quary>

VS Code extension: <https://marketplace.visualstudio.com/items?itemName=Quary.quary-extension>

## Summary

Quary is an open-source SQL transformation framework — think **dbt rewritten
in Rust with a VS Code extension as the primary interface**. You define
sources, models (SQL transformations), tests, and charts as code, version
them in Git, and execute them against a target database. It ships a
DuckDB-backed local development mode so you can `quary init` a sample
project and have something running in seconds.

## Insight

Where Quary fits in the dbt-alternatives landscape:

- **vs dbt Core** — dbt is the incumbent, Python-based, with a vast plugin
  ecosystem. Quary is younger, Rust-based, faster to start, and has a
  built-in IDE story. If your team lives in dbt already, the plugin breadth
  is a strong reason to stay; if you're greenfield and value local-first
  + fast feedback, Quary is worth a serious look.
- **vs SQLMesh** — SQLMesh focuses on virtual data environments and proper
  semantic versioning of models (vs dbt's "rebuild everything"). It's the
  more sophisticated alternative for teams hitting dbt's incremental-model
  pain. Quary is closer to dbt in feature shape but with a different UX.
- **vs Quary's own ambition** — the project also wants to cover charts and
  dashboards (BI-shaped) which dbt deliberately doesn't. Whether that's a
  strength or scope creep depends on your taste.

When to reach for Quary: small-to-mid teams that want SQL-as-code, prefer a
Rust CLI to a Python one, and are happy with a tighter VS Code feedback loop
than dbt's Python REPL. Less obvious for huge dbt-mature shops.

## Quick start

Quary is a VS Code extension (UI) backed by a Rust CLI (core). The CLI is
required.

Install the CLI:

```sh
# Homebrew
brew install quarylabs/quary/quary

# Linux/macOS via curl
curl -fsSL https://raw.githubusercontent.com/quarylabs/quary/main/install.sh | bash
```

Initialise and build a sample project:

```sh
mkdir example && cd example
quary init      # initialise a DuckDB demo project with sample data
quary compile   # validate project structure / model references (no DB needed)
quary build     # build views/seeds against the target database
quary test -s   # run tests against the target database
```

## Asset types

Defined and managed as code in the project:

- **Sources** — external inputs: database tables, flat files, APIs (DuckDB
  reads many of these directly).
- **Models** — SQL transformations that turn sources into analysis-ready
  datasets, splittable into atomic components.
- **Charts** — visual representations defined in SQL.
- **Dashboards** *(WIP)* — multiple charts in one view.
- **Reports** *(WIP)* — packaged insights to share.

## Similar / related topics

- **dbt Core** — the incumbent SQL-as-code framework.
  <https://github.com/dbt-labs/dbt-core>
- **SQLMesh** — semantic-versioning-aware dbt alternative.
  <https://sqlmesh.com/>
- **Dataform** — Google-acquired, BigQuery-flavoured.
- **Lea** — minimal Python-based alternative.
- **DuckDB** — the engine Quary's local mode runs on.

## Internal links

- [[db/tools/README|tools]] — section landing.
- [[sqlflow]] — streaming-SQL sibling tool.
- [[indexing]] — index design for the queries Quary generates.
- [[db/analytics/README|analytics]] — broader analytics context.

## Keywords

`#quary` `#sql` `#dbt-alternative` `#transformation` `#duckdb` `#vscode` `#rust` `#models`

## References / raw notes

- Repo: <https://github.com/quarylabs/quary>
- VS Code extension:
  <https://marketplace.visualstudio.com/items?itemName=Quary.quary-extension>
- Releases (other build targets):
  <https://github.com/quarylabs/quary/releases>
