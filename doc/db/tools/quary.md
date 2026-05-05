---
title: Quary
main_link: https://github.com/quarylabs/quary
keywords: [quary, tools, db, sql, cli, databases]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Quary

**Main link:** <https://github.com/quarylabs/quary>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[sqlflow]] — SQLFlow _(score 18)_
- [[indexing]] — Indexing _(score 18)_
- [[demo_overview]] — DEMO _(score 15)_
- [[db]] — diesel _(score 15)_
- [[sql_pen_test]] — sqlmap _(score 13)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#quary` `#tools` `#db` `#sql` `#create` `#cli` `#database`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Quary


https://github.com/quarylabs/quary


## With Quary, engineers can:
- 🔌 Connect to their Database
- 📖 Write SQL queries to transform, organize, and document tables in a database
- 📊 Create charts, dashboards and reports (in development)
- 🧪 Test, collaborate & refactor iteratively through version control
- 🚀 Deploy the organised, documented model back up to the database


## Asset Types in Quary
Define and manage the following asset types as code:

- Sources: Define the external data sources that feed into Quary, such as database tables, flat files, or APIs (with DuckDB).
- Models: Transform raw data from sources into analysis-ready datasets using SQL, this lets engineers split complex queries into atomic components.
- Charts: Create visual representations of your data using SQL.
- 🚧 Dashboards (WIP): Combine multiple charts into a single view, allowing engineers to monitor and analyze data in one place.
- 🚧 Reports (WIP): Create detailed reports to share insights and findings with your team or stakeholders.

## 🚀 Getting Started
### Installation
Quary is a VSCode Extension (Interface) & Rust-based CLI (Core)

### Extension
The VSCode extension can be installed here. Note that it depends on the CLI being installed.

[https://marketplace.visualstudio.com/items?itemName=Quary.quary-extension](https://marketplace.visualstudio.com/items?itemName=Quary.quary-extension)

### CLI

Homebrew installation
```shell
brew install quarylabs/quary/quary
```
Linux/Mac through curl
Quary can be installed using curl on Linux/Mac using the following command:

```shell
curl -fsSL https://raw.githubusercontent.com/quarylabs/quary/main/install.sh | bash
```

Other installations
Other builds are available in the releases page to download.

## Usage
Once installed, a sample project can be created and run as follows:
```shell
mkdir example # create an empty project folder
cd example
quary init    # initialize DuckDB demo project with sample data
quary compile # validate the project structure and model references without database
quary build   # build and execute the model views/seeds against target database
quary test -s   # run defined tests against target database
```
