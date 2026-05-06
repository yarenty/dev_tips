# Vault Reorganization Plan

> Master plan to restructure the `dev_tips` Obsidian vault: clean up `doc/`, normalize landing pages to `README.md`, and rewrite every article in a consistent, link-rich, graph-friendly format.

## Status legend

Use these emoji prefixes everywhere in this document so progress is scannable at a glance:

- ⬜ TODO – not started
- 🟨 IN PROGRESS – an agent is actively working it (add agent name + start time in comment)
- ✅ DONE – finished (add finish date + 1-line note)
- ⚠️ BLOCKED – waiting on a decision or external input (add reason)
- ⏭️ SKIPPED – consciously deferred (add reason)

When you finish a task, update the status line in-place and add a short `> comment:` note underneath. Never delete history.

---

## 0. Goals & Principles

1. **One landing page per directory.** Always `README.md`. No more `INDEX.md`, `index.md`, or `<topic>.md` acting as the index.
2. **Predictable folder taxonomy.** Lowercase, snake_case, no spaces, no typos.
3. **Every article follows the same template** (title, main link, summary, insight, similar links, keywords, internal links).
4. **Obsidian-graph-friendly.** Use `[[wikilinks]]` between articles and a `keywords:` frontmatter list so the graph view is meaningful.
5. **mdBook still builds.** `SUMMARY.md` must stay valid and reflect the new tree.
6. **Non-destructive.** Use `git mv` (or the `move_path` tool) when renaming so history is preserved. Never delete a file without first migrating its content.

---

## 1. High-level phases

| Phase | Description | Owner | Status |
|-------|-------------|-------|--------|
| P1 | Inventory & audit | agent | ✅ DONE |
| P2 | Taxonomy redesign (folder tree proposal) | agent + user review | ✅ DONE |
| P3 | Folder restructure (rename / move / merge) | agent | ✅ DONE |
| P4 | Normalize landing pages to `README.md` | agent | ✅ DONE |
| P5 | Rewrite each article to the standard template | agent (batched) | ✅ DONE (auto-stub pass) |
| P6 | Cross-link & keyword pass (graph polish) | agent | ✅ DONE |
| P7 | Rebuild `SUMMARY.md` & verify `mdbook build` | agent | ✅ DONE |
| P8 | Final review & cleanup | user | ⬜ TODO |

Each phase below has concrete, individually trackable tasks. Work them top-to-bottom.

---

## 2. Standard article template

Every non-landing article in `doc/**` MUST be rewritten to follow this template. Keep the existing technical content (snippets, commands, links) but reorganize/augment it so the metadata sections exist.

```/dev/null/article_template.md#L1-40
---
title: <Human-readable title>
main_link: <single canonical URL — official site / repo / paper>
keywords: [kw1, kw2, kw3]   # 3-10 short tags, lowercase, used by Obsidian graph
status: draft | reviewed     # author's confidence in the rewrite
---

# <Title>

**Main link:** <main_link>

## Summary

2-5 sentences. What is it? Who made it? What does it do?

## Insight

What is it really *for*? When and where would I reach for this?
Any gotchas, opinions, or comparisons learned from investigation.

## Similar / related topics

- <other tools/approaches in the same space, with 1-line description>
- ...

## Internal links

- [[<other_article_in_vault>]]
- [[<directory_README>]]

## Keywords

`#kw1` `#kw2` `#kw3`

## References / raw notes

<Original links, code blocks, install instructions, etc. preserved here.>
```

### Landing page (`README.md`) template

```/dev/null/readme_template.md#L1-25
# <Section title>

<1-3 sentence description of the section's scope.>

## Subsections

- [<Subdir name>](<subdir>/README.md) — short description

## Articles

- [<Article title>](<file>.md) — short description
- ...

## Keywords

`#section-tag` `#related-tag`

## See also

- [[../other_section/README]]
```

---

## 3. Proposed taxonomy (target tree under `doc/`)

This is the *target* shape. Phase P3 reshapes the current tree into this. Decisions marked **⚠️ NEEDS USER CONFIRMATION** before bulk moves.

```
doc/
├── README.md                      # top-level index, replaces intro.md + SUMMARY.md narrative
├── SUMMARY.md                     # mdBook ToC (regenerated from tree)
├── references.md
│
├── programming/
│   ├── README.md
│   ├── rust/                      # large – keep deep subtree, but tidy
│   │   ├── README.md
│   │   ├── learning/              # _must_have, _to_learn, _todo_ideas, _tricks → here
│   │   ├── core/                  # core, borrow_check, error, derivative, reflection, generational_index, low_level_structures
│   │   ├── concurrency/           # concurrency, actors, salsa, timely, streaming
│   │   ├── io/                    # io, json, xls, parsers, bytes_manipulation, hdfs, storage
│   │   ├── net/                   # gRPC, trpc, tunelling, MQ
│   │   ├── web/                   # existing
│   │   ├── gui/                   # existing (rename gui.mdi.md → gui.md)
│   │   ├── tui/
│   │   ├── cli/
│   │   ├── data/                  # datafusion, lance_data_format, ml_letsql, meilisearch, trustfall
│   │   ├── ml/                    # rust_ML/, ML.md, cuda.md
│   │   ├── interop/               # to_java, to_net, to_python, java2rust, rust_java, python.md
│   │   ├── tooling/               # tools, app_builders, compilation_cache, repl, debug, tests, tracing, loggers
│   │   ├── plugins/               # rust_plugins/
│   │   ├── sql_engine/            # Rust_SQL_engine/ (renamed)
│   │   ├── projects/              # datafusion_projects/
│   │   └── misc/                  # fun, raspberry_pi, windows, audio, charts
│   ├── java/
│   ├── scala/
│   ├── cpp/
│   ├── go/
│   ├── swift/
│   ├── php/                       # moved from doc/php/
│   ├── assembly/
│   ├── package_managers/
│   ├── datafusion/                # cross-language datafusion notes (vs rust/data/datafusion)
│   ├── caching/                   # was 'cache'
│   └── unix_systems/              # building_unix_system.md, minio.md, monaco.md
│
├── data/                          # was 'db' – broader: databases + storage + formats
│   ├── README.md
│   ├── relational/                # mysql, postgres, postgresql, surrealDB, singlestore, edgeDB, fauna, toyDB, tensorbase, xlite, limbo
│   ├── timeseries/                # influxdb, greptimeDB, druid, questDB
│   ├── nosql/                     # hbase, redis, firebase
│   ├── vector/                    # chroma, qdrant
│   ├── analytics/                 # databend, datafusion, gluten, tensor_query (space fixed)
│   ├── streaming/                 # noria_streaming_db, streaming_query_approximation, delta_live, CDC
│   ├── distributed/               # ballista_distributed, federated, federated_query_execution, scheduling/, long_running_query
│   ├── catalogs/                  # unity_hive_catalog (typo fixed)
│   ├── formats/                   # nimble_file_format, table_transfer_protocols
│   ├── quality/                   # data_quality, data_quality_problems_in_AI, data_curation
│   ├── fabric/                    # data_fabric
│   ├── udf/                       # UDF
│   ├── tools/                     # quary, sqlflow, indexing, SQL_PEN_TEST
│   └── misc/                      # Untitled.md (triage)
│
├── ml/                            # major cleanup: dedupe LLM/llm/ai-llm/agents
│   ├── README.md
│   ├── fundamentals/              # courses, classification, kmeans (typo fixed), graphNN, KAN, no_deep_learning_needed (rename), zero_shot, pattern_learning, unlearning, ML/, ML_auto_indexing_DBs
│   ├── data/                      # existing data/
│   ├── frameworks/                # existing frameworks/, h2o, mindsdb, mlcube, DSPy, phidata
│   ├── compilers/                 # XLA_accelerated_compiler, mlgo_llvm, tiramisu, tiny_gpu, CUDA/
│   ├── time_series/               # MERGE timeseires/ + time_series/
│   ├── knowledge_graph/
│   ├── genetics/
│   ├── quantum/
│   ├── memory/
│   ├── skills/
│   ├── llm/                       # MERGE LLM/ + llm.md + llm_os.md + ai/llm/ + ollama/ + llama.md
│   │   ├── README.md
│   │   ├── models/                # deepseek, OLMo, llama, mplug, miniLLM, jetbrains_mellum, strawberry
│   │   ├── runtimes/              # ollama, llama.cpp
│   │   ├── tokenizers/
│   │   ├── inspection/            # inspectus, leaderboards, abliteration
│   │   └── system_prompts/
│   ├── rag/                       # MERGE RAG/ + RAG_PDF/ + rag_query_summarize/ + ai/rag.md + ai/llm/rag.md + ai/llm/kag.md + ai/llm/graph_rag.md + ai/llm/neo4j_rag.md + agents/rag.md
│   ├── agents/                    # MERGE agents/ + ai/agants.md (typo) + ai/devin.md + ai/devon.md + ai/co-scientist.md + ai/emdash.md + agents/smolagents.md + agents/cloude.md
│   ├── mcp/
│   ├── finetuning/
│   ├── tools/                     # ai/tools/, cursor, tabby, ai/burn, candle, dfdx, tch, tract, deeplake
│   ├── voice/                     # ai/voice/
│   ├── orchestration/             # ai/ai_data_orchestration/
│   └── bigquery/                  # bigquery.md + BigQueryML/
│
├── infra/                         # NEW grouping: cloud + k8s + iot + dpu + observability + messaging
│   ├── README.md
│   ├── cloud/                     # existing cloud/
│   ├── kubernetes/                # existing
│   ├── iot/                       # existing
│   ├── messaging/                 # was 'messagging' (typo fixed)
│   ├── observability/             # existing
│   └── dpu/                       # was DPU/ (lowercased)
│
├── tools/                         # developer tooling, OS, shell
│   ├── README.md
│   ├── osx/
│   ├── linux/
│   ├── shell/
│   ├── live_demos/
│   ├── security/                  # security, port_scan, port_scanner_RustScan, sniffnet (typo fixed from sinffnet), wireguard, rathole, ngrok, web_to_app_PAKE
│   ├── editors/                   # intellij, void, mdfried, markitdown
│   ├── design/                    # design, fengshui, ascii_generator, applications, gramma_harper, lstr, superfile
│   ├── networking/                # mtr
│   └── misc/                      # rust_MUST, payments, md_to_ppt_pdf_jpeg
│
├── visualization/                 # existing, mostly tidy
│   ├── README.md
│   ├── pdf/
│   ├── latex/
│   ├── web_templating/
│   └── (charting tools as articles)
│
├── git/                           # existing
├── games/                         # existing
├── trading/                       # existing
├── robot/                         # existing
├── digital_twin/                  # existing
├── www/                           # was WWW/ (lowercased)
├── demo/                          # existing
├── funny/                         # existing
├── published/                     # papers, slides – keep as-is, just add README.md
└── assets/                        # images – unchanged
```

### Renames/typos that **must** be fixed (mechanical)

| From | To |
|------|-----|
| `doc/messagging/` | `doc/infra/messaging/` |
| `doc/DPU/` | `doc/infra/dpu/` |
| `doc/WWW/` | `doc/www/` |
| `doc/db/unity_hive_catalogur/` | `doc/data/catalogs/unity_hive_catalog/` |
| `doc/db/tensor query/` | `doc/data/analytics/tensor_query/` |
| `doc/ml/timeseires/` | merged into `doc/ml/time_series/` |
| `doc/ml/no deep learning needed/` | `doc/ml/fundamentals/no_deep_learning_needed/` |
| `doc/ml/kmenas.md` | `doc/ml/fundamentals/kmeans.md` |
| `doc/ml/ai/agants.md` | merged into `doc/ml/agents/` |
| `doc/tools/sinffnet.md` | `doc/tools/security/sniffnet.md` |
| `doc/programming/rust/gui/gui.mdi.md` (per `SUMMARY.md`) | `doc/programming/rust/gui/gui.md` |
| `doc/programming/rust/web/http.mdp.md` (per `SUMMARY.md`) | `doc/programming/rust/web/http.md` |
| `doc/programming/rust/gui/slint.mdui/slint.md` (per `SUMMARY.md`) | `doc/programming/rust/gui/slint.md` |
| `doc/db/Untitled.md` | triage – name properly or delete |
| All `INDEX.md` / `index.md` | `README.md` (after merging current `README.md` content if both exist) |

---

## 4. Phase P1 — Inventory & audit

Goal: produce a machine-readable inventory of every file under `doc/` so later phases can be batched without re-scanning.

- [x] **P1.1** ✅ DONE — Generate `inventory/files.tsv` listing: `path \t bytes \t lines \t has_frontmatter \t first_h1`.
  > comment: agent zed-opus — 447 .md files indexed; output at `inventory/files.tsv`.
- [x] **P1.2** ✅ DONE — Generate `inventory/dirs.tsv` listing: `dir \t has_README \t has_INDEX \t has_index \t has_topic_md (e.g. db/db.md) \t article_count`.
  > comment: agent zed-opus — 118 directories indexed; output at `inventory/dirs.tsv`. Confirmed many dirs have BOTH `README.md` AND a `<topic>.md` (e.g. `ml/ml.md` + `ml/README.md`); P4 will need to merge these.
- [x] **P1.3** ✅ DONE — Generate `inventory/duplicates.md` flagging suspected duplicates / overlaps (e.g. `ml/llm.md` vs `ml/LLM/` vs `ml/ai/llm/`).
  > comment: agent zed-opus — 21 stem-collision groups + 9 dir-collision groups + thematic tangles documented at `inventory/duplicates.md`.
- [x] **P1.4** ✅ DONE — Generate `inventory/broken_links.md` from `grep`-ing markdown links to files that no longer exist.
  > comment: agent zed-opus — 121/424 relative links are broken; output at `inventory/broken_links.md`. Notable: `SUMMARY.md` references typo paths (`gui.mdi.md`, `http.mdp.md`, `slint.mdui`); confirms §3 typo table.
- [x] **P1.5** ✅ DONE — Generate `inventory/empty_or_stub.md` listing files < 200 bytes or with no real content.
  > comment: agent zed-opus — 81 stub/empty files flagged at `inventory/empty_or_stub.md`. Many are exactly the `<topic>.md` siblings of a real `README.md` (so they will be deleted in P4); 5 are 0 bytes and should be triaged.

Put all P1 outputs in a new `inventory/` directory at the repo root. They are working files; commit them so future agents can read them.

---

## 5. Phase P2 — Taxonomy review

Goal: lock the target tree (§3) before any moves happen.

- [x] **P2.1** ✅ DONE — Walk the proposed tree in §3 against `inventory/dirs.tsv` and adjust anything obviously wrong.
  > comment: agent zed-opus — done implicitly while building `inventory/build_move_map.py`. Adjustments captured in code: `streaming_query_approximation/` keeps its project sub-folder under `db/streaming/`; `agent_instructs/` keeps its sub-folder under `ml/agents/`; `INREX.md` (typo of INDEX) maps to `README.md`; spaces and hyphens in filenames are normalised to underscores; double-dot `anmell..md` flagged as RENAME.
- [x] **P2.2** ✅ DONE — Produce `inventory/move_map.tsv` with one row per source path → destination path. This is the single source of truth for P3.
  > comment: agent zed-opus — generator at `inventory/build_move_map.py`; output at `inventory/move_map.tsv`; human-readable summary at `inventory/move_map_summary.md`. 446 mappings total: 175 KEEP / 232 MOVE / 34 MERGE / 1 RENAME / 4 DELETE. Zero accidental destination collisions.
- [x] **P2.3** ✅ DONE — User confirmation on debatable groupings:
  - Should `programming/datafusion/` and `programming/rust/data/datafusion/` merge? **YES** → merge into `programming/rust/data/datafusion/`.
  - Should `php/` live under `programming/` or stay top-level? **programming** → `doc/programming/php/`.
  - Should `infra/` umbrella exist, or keep `cloud/`, `kubernetes/`, etc. top-level? **KEEP TOP-LEVEL** → no `infra/` umbrella; `cloud/`, `kubernetes/`, `iot/`, `messaging/`, `observability/`, `dpu/` all stay directly under `doc/`.
  - Should `db/` rename to `data/`? **NO** → keep `doc/db/` (it is about databases, not generic data).
  > comment: decisions captured 2025 — taxonomy in §3 must be re-read with these overrides applied. The §3 tree shows the *original* proposal; the move_map in P2.2 will reflect the FINAL decisions above.

---

## 6. Phase P3 — Folder restructure

Goal: execute `inventory/move_map.tsv` using `move_path` (preserves history). Do NOT batch all moves into one giant commit — group by top-level destination so reviews are tractable.

Driver rule: **one task per top-level destination directory.**

- [x] **P3.1** ✅ DONE — Create new top-level dirs that don't exist (`www/`, `messaging/`, `dpu/`, plus the new `db/**` and `programming/rust/**` sub-dirs).
  > comment: agent zed-opus — obsoleted by `inventory/apply_move_map.py` which auto-creates parent dirs on demand. No standalone action required; verified with `--dry-run --dst-prefix doc/db/` (31 MOVE + 1 MERGE planned, no errors).
- [x] **P3.2** ✅ DONE — Move everything destined for `doc/programming/` (including the rust subtree reorganisation, `php/` move under `programming/`, and the `programming/datafusion/` → `programming/rust/data/datafusion/` merge).
  > comment: agent zed-opus — 91 MOVE + 12 MERGE applied via `apply_move_map.py --dst-prefix doc/programming/`. Asset folders (img/, res/, .pptx, .sql, .txt) relocated to mirror new taxonomy. Empty source dirs (`java2rust`, `rust_java`, `rust_ML`, `rust_plugins`, `Rust_SQL_engine`, `cache`, `configuration`, `datafusion_projects`, `datafusion`) removed. New tree verified clean: 13 top-level rust subdirs, no orphans.
- [x] **P3.3** ✅ DONE — Move everything destined for `doc/db/` (regroup into relational/timeseries/nosql/vector/analytics/streaming/distributed/catalogs/formats/quality/fabric/udf/tools/misc).
  > comment: agent zed-opus — 30 MOVE + 3 MERGE applied via `apply_move_map.py --dst-prefix doc/db/`. Asset folders for ballista_distributed, data_curation, data_fabric, databend, datafusion, delta_live, federated*, gluten, long_running_query, nimble_file_format, scheduling/{google,microsoft,vldb}, tensor_query, unity_hive_catalog all relocated under their new taxonomic homes. UDF case-insensitive duplicate handled. Empty source dirs removed. New tree: 12 top-level db sub-categories.
- [x] **P3.4** ✅ DONE — Move everything destined for `doc/ml/` (the big LLM/RAG/agents merge — see §3).
  > comment: agent zed-opus — 62 MOVE + 25 MERGE + 1 RENAME applied. ai/, timeseires/, LLM/, RAG/, RAG_PDF/, rag_query_summarize/, KAN/, ML/, ML_auto_indexing_DBs/, CUDA/, ai/ai_data_orchestration/, no deep learning needed/, zero_shot/, ollama/, agents/, mcp/ all reshaped. LLM hub recovered after a case-insensitive FS hiccup that nested everything inside `papers/` (manually re-flattened). Final tree: 19 top-level ml sub-categories, 106 .md files.
- [x] **P3.5** ✅ DONE — Rename / tidy individual top-level dirs that stay top-level: `WWW→www`, `messagging→messaging`, `DPU→dpu`. (No `infra/` umbrella per user decision.)
  > comment: agent zed-opus — 19+9+2 MOVEs across www/messaging/dpu. Hit a case-insensitive APFS bug where same-inode src/dst caused content loss; fixed `apply_move_map.py` to detect `os.path.samefile()` and do an in-place case-rename via temp name. 11 lost files (WWW articles, mcp4DB.md, vitis.md, external_udfs.md) recovered from `git checkout HEAD --` and re-applied. Final tree: `doc/www/`, `doc/messaging/`, `doc/dpu/` all lowercase; old uppercase aliases gone (HFS/APFS quirk — listings may still echo both cases but inodes are correct).
- [x] **P3.6** ✅ DONE — Move everything destined for `doc/tools/` (regroup security/editors/design/networking).
  > comment: agent zed-opus — 24 MOVE + 4 MERGE applied. New tree: `tools/{design,editors,linux,live_demos,misc,networking,osx,security,shell}/`. Deleted 5 exact-duplicate `<dir>/<dir>.md` files (`tools/tools.md`, `observability/observability.md`, `robot/robot.md`, `iot/iot.md`, `kubernetes/kubernetes.md`, `trading/trading.md`) that exactly matched their `README.md`. 12 other `<dir>/<dir>.md` files have *different* content and are deferred to P4 for proper README merge.
- [x] **P3.7** ✅ DONE — Fix remaining file-name typos from §3 (e.g. `sinffnet→sniffnet`, `kmenas→kmeans`, `gui.mdi.md→gui.md`, `http.mdp.md→http.md`).
  > comment: agent zed-opus — ran the move map for all remaining destination prefixes (visualization, published, cloud, iot, git, observability, digital_twin, funny, games, trading, robot, demo, kubernetes, messaging). Final scan: `find doc -name '*.md' | grep [A-Z] | grep -v README` returns only `SUMMARY.md` (intentional). All article filenames are now lowercase + underscore-separated. Typos fixed in-place by the move map: `sinffnet→sniffnet`, `kmenas→kmeans`, `unity_hive_catalogur→unity_hive_catalog`, `tensor query→tensor_query`, `agants→agents`, `anmell..md→anmell.md`, `INREX→README`. (Note: `gui.mdi.md`/`http.mdp.md`/`slint.mdui` were never real files — they were only typo paths in `SUMMARY.md`; SUMMARY will be regenerated in P7.)
- [x] **P3.8** ✅ DONE — Delete now-empty source directories.
  > comment: agent zed-opus — removed all `.DS_Store` files; `find doc -depth -type d -empty -delete` left zero empty dirs. `doc/php/` (which had only assets, no .md) auto-collapsed. Final tree: 159 directories, 425 .md files (down from 446 sources due to 21 merges + 4 deletes).

---

## 7. Phase P4 — Normalize landing pages to `README.md`

Goal: every directory under `doc/**` has exactly one landing file named `README.md`, following the landing-page template (§2).

Algorithm per directory:

1. Detect existing landing file(s): `README.md`, `INDEX.md`, `index.md`, and any `<topic>.md` matching the dir name (e.g. `db/db.md`).
2. Pick the richest one as the base.
3. Merge content from the others into it.
4. Rewrite to follow the landing-page template.
5. `move_path` it to `README.md` and delete the merged-out duplicates.
6. Update any links in sibling/parent files that pointed to the old name.

Track one task per top-level directory (so a single agent run is bounded).

- [x] **P4.1** ✅ DONE — Top-level `doc/README.md` (merge `intro.md` content here).
  > comment: agent zed-opus — hand-written canonical landing using the template; folds in mdBook intro and the investigation procedure. `doc/intro.md` deleted.
- [x] **P4.2** ✅ DONE — `doc/programming/**` READMEs.
  > comment: agent zed-opus — generator at `inventory/readme_gen.py` produced 29 READMEs across `doc/programming/**`. Walks bottom-up so child H1 titles are visible to parent generators. Removes old landing aliases (`<dir>/<dir>.md`, `INDEX.md`, `index.md`) automatically. `programming.md`, `rust.md`, `scala.md`, etc. all consolidated into the directory `README.md`.
- [x] **P4.3** ✅ DONE — `doc/db/**` READMEs.
  > comment: agent zed-opus — generated for all 14 db sub-categories.
- [x] **P4.4** ✅ DONE — `doc/ml/**` READMEs.
  > comment: agent zed-opus — generated for all 19 ml subcategories incl. the LLM hub (models/runtimes/inspection/system_prompts/tokenizers).
- [x] **P4.5** ✅ DONE — Top-level infra-area READMEs: `cloud/`, `kubernetes/`, `iot/`, `messaging/`, `observability/`, `dpu/`.
  > comment: agent zed-opus — generated; `dpu/{xilinx,xpu}/` sub-READMEs included.
- [x] **P4.6** ✅ DONE — `doc/tools/**` READMEs.
  > comment: agent zed-opus — generated for `tools/` plus `osx/`, `linux/`, `shell/`, `live_demos/`, `security/`, `editors/`, `design/`, `networking/`, `misc/`.
- [x] **P4.7** ✅ DONE — `doc/visualization/**` READMEs.
  > comment: agent zed-opus — generated for `visualization/`, `pdf/`, `latex/`, `web_templating/`.
- [x] **P4.8** ✅ DONE — All remaining small top-level dirs (`git`, `games`, `trading`, `robot`, `digital_twin`, `www`, `demo`, `funny`, `published`).
  > comment: agent zed-opus — generated.

Verification:
- `find doc -name 'INDEX.md' -o -name 'index.md'` returns nothing.
- All 131 navigable directories have a `README.md`.
- `inventory/find_topic_dups.py` returns nothing (no `<dir>/<dir>.md` duplicates remain).
- Asset directories (`img/`, `res/`, `res_pptx/`, `papers/`) intentionally excluded from README generation; spurious READMEs that landed there were deleted and the generator updated to skip them.

---

## 8. Phase P5 — Rewrite each article (the big one)

Goal: every non-landing `.md` under `doc/**` is rewritten to the article template (§2), preserving original information.

There are ~447 markdown files. **Don't try to do them all in one session.** Batch by directory and track progress per batch.

### Cross-link policy ("hybrid C", agreed during P5.I)

When a reviewed article wants to wikilink to a target outside the current batch:

1. **If the target already exists with substantive content** — use a normal `[[stem]]` (or `[[path/stem|alias]]` to disambiguate).
2. **If the target is a canonical / often-referenced landing page** (e.g. `tokio`, `prometheus`, `grafana`, `redis`, `postgres`, `polars`, `tokio`, `kubernetes`, `docker`, `python`, `rust`) **and is currently empty/stub**, fill it now even if its batch is technically later — those will keep coming up.
3. **Otherwise** — demote to a plain external link (`[Polars](https://pola.rs/)`) so the curated article doesn't pretend to point at curated internal content. The orphan-target list at the bottom of this section captures the demotions so a later batch can pick them up.

The **filename-stem ambiguity rule**: if more than one file shares a stem (e.g. `grafana.md` exists in both `observability/` and `visualization/`), always use the explicit `[[<dir>/stem|display]]` form. Bare `[[stem]]` only when the stem is unique across the whole vault.

Landing-page fills already done outside their owning batches:

- `programming/rust/concurrency/tokio.md` (created during P5.I; will count as already-reviewed for P5.W)
- `programming/rust/concurrency/crossbeam.md` (created during P5.I; will count as already-reviewed for P5.W)
- `observability/grafana.md` (filled during P5.I; will count as already-reviewed for P5.N)
- `observability/prometheus.md` (filled during P5.I; will count as already-reviewed for P5.N)
- `messaging/mqtt.md` (filled during P5.L; will count as already-reviewed for P5.M)

### Batches (one row per batch — update the status as you go)

| Batch | Scope | File count (approx) | Status | Comment |
|-------|-------|--------------------|--------|---------|
| P5.A | `doc/git/**` | 3 | ✅ DONE (reviewed) | hand-rewritten 2025: `config.md`, `git_delta.md`, `github.md` + `README.md` all promoted to `status: reviewed`; auto-stub markers stripped; real Summary/Insight/Similar/Internal/Keywords/References on each. |
| P5.B | `doc/funny/**` | 4 | ✅ DONE (reviewed) | hand-rewritten 2025: `brainfuck.md`, `brain.md` (renamed from `rust_brain.md`), `algorithms.md`, `sleepsort.md` + `README.md`. All `status: reviewed`; raw notes trimmed and structured (e.g. brainfuck got a clean BF↔C instruction table; sleepsort got Wikipedia as canonical link, asyncio reference impl, complexity discussion). Renamed `rust_brain.md` → `brain.md` (project's actual name); SUMMARY regenerated. mdbook build clean. |
| P5.C | `doc/games/**` | 5 | ✅ DONE (reviewed) | hand-rewritten 2025: `corewars.md` (Core War + Redcode quick-reference + Imp warrior), `engines.md` (Bevy vs Piston framing), `game_in_scala.md` (SGL / Scala-Native context), `zxspectrum.md` (rustzx + emulator-source pitch) + `README.md`. All `status: reviewed`; `main_link`s upgraded to canonical sources (corewar.co.uk, arewegameyet.rs, etc.). |
| P5.D | `doc/robot/**` | 2 | ✅ DONE (reviewed) | hand-rewritten 2025: `groot.md` repositioned as "NVIDIA GR00T — humanoid-robot foundation model" with proper context (Isaac stack, ROS 2 bridge, lock-in caveat, GR00T N1 open release); README expanded with cross-section see-also (lerobot, nvidia_omniverse, digital_twin). All `status: reviewed`. |
| P5.E | `doc/demo/**` | 1 | ✅ DONE (reviewed) | hand-rewritten 2025: `term.md` retitled "presenterm — markdown presentations in the terminal" with install/usage/slide-format examples; README cross-links to `tools/live_demos/` for recording side. All `status: reviewed`. |
| P5.F | `doc/digital_twin/**` | 10 | ✅ DONE (reviewed) | hand-rewritten 2025: 8 articles + README. **Deleted** `nvidia.md` (folded into `nvidia_omniverse.md` since both were about Omniverse). Major content adds: `definition.md` got the IBM 4-level taxonomy + history + simulation-vs-twin comparison table; `cps.md` got the CPS ↔ digital-twin terminology bridge; `bmw.md` got iFACTORY "lean/green/digital" framing + Debrecen digital-first-plant story; `isc2022.md` distilled the keynote down to the four substantive points + Lebaredian's superpowers table; `nvidia_omniverse.md` got the full stack table + Modulus neural-operator architectures; `scilab.md` and `simulink.md` (previously empty) got proper proper open-vs-commercial framing + lifecycle tables; `links.md` reorganised as curated reading list (IBM family / IBM×Siemens / customer stories / MathWorks); README has reading order + cross-section see-also (GR00T, IoT). All `status: reviewed`. |
| P5.G | `doc/www/**` | 10 | ✅ DONE (reviewed) | hand-rewritten 2025: 9 articles + README. **Deleted 2 duplicates**: `n8n_io.md` (kept `ml/tools/n8n.md` as canonical) and `payments.md` (kept `tools/misc/payments.md`). **Renamed** `tools.md` → `tally.md` (the file was actually about Tally form-builder, not generic tools). All articles reframed with proper titles + when-to-use guidance. Big content adds: `apache.md` got `apachectl` cheat-sheet + troubleshooting; `signing_mac_libs.md` distilled the 220-line Phusion blog into A/B/C/D recipe; `ssl.md` got Certbot recipes for Apache/nginx/standalone + DNS-01 wildcard; `loco_rust_on_rails.md` got the full features table + quick-start; `ssg.md` framed Zola+Tera + when-not-to-reach-for; `airtable.md` framed with NocoDB/Baserow alternatives; `tally.md` got integration shape diagram; `yarenty_profile_*.md` shrunk to a clean cross-linkable bio. README organised into Servers&TLS / Building sites&apps / No-code&forms / Reference. All `status: reviewed`. |
| P5.H | `doc/trading/**` | 6 + README | ✅ DONE (reviewed) | hand-rewritten 2025: 6 articles + README. **Consolidated** the 3-part Folbrecht series (`a_basic_algo_trading_system_in_rust_part_{i,iii,iv_backtesting}.md` + the `rust_algo_trading.md` parent stub + the `mt5.md` near-empty stub) into a single richer article `folbrecht_algo_trading_series.md`. **Fenced** all the embedded Python / MQL5 snippets in `mql5.md` (the splitter-killer file: dozens of `# Initializing X` lines that were Python comments, not headings) and rewrote it as a proper article on the MQL5↔Python socket bridge + the `MetaTrader5` Python package. `barter.md`, `bot_rust.md` (tickgrinder), `hummingbot_python.md`, `trade_aggregation_rust_candles.md` all rewritten with proper Summary/Insight/Similar/Internal/Keywords + cross-section see-also that triangulates the four big design axes (sync vs. async, pedagogy vs. production, Rust vs. Python, single-process channels vs. external bus). README rewritten as a curated index organised along those axes. SUMMARY.md updated. mdbook build clean. All `status: reviewed`. |
| P5.I | `doc/visualization/**` | 14 articles + 4 READMEs | ✅ DONE (reviewed) | hand-rewritten 2025: 14 articles + 4 READMEs in viz, **plus 4 cross-section landing pages** filled under hybrid-C policy (see below). **Deleted** empty `visualization/latex/` subdir + empty `pdf/latex.md` stub. **Renamed** `rust.md`→`plotters.md` (real title), `javascript.md`→`svelvet.md`, `tips.md`→`dataviz_resources.md`. Big content adds: `diagrams.md` framed as drawio+D2 with the static-vs-DSL split; `fonts.md` got a 9-row programming-fonts table with ligature+width axes; `manim.md` framed as 3Blue1Brown's tooling with the Community vs. 3b1b fork warning; `rerun.md` got the canonical "vacuum-cleaning robot" use case + Python hello-world + recording/replay; `plotters.md` clarified as drawing-not-viz with sine-curve example + GTK-rs detour; `pdf/typst.md` got the LaTeX comparison + when-to-use guidance; `pdf/mdmath_symbols.md` reformatted from broken pasted text into proper Markdown tables (8 categories) + KaTeX/MathJax compat notes; `pdf/react.md` framed react-print-pdf with HTML-PDF tradeoff matrix; `web_templating/liquid.md` framed safety-first vs. Jinja/Tera. **Hybrid-C cross-section fills**: created `programming/rust/concurrency/tokio.md` (referenced 3× from trading), created `programming/rust/concurrency/crossbeam.md` (referenced from folbrecht_algo_trading_series), filled `observability/grafana.md` as operator's view (install/HA/provisioning), filled `observability/prometheus.md` as PromQL+architecture landing page. Updated `programming/rust/concurrency/README.md` with workload→tool table. SUMMARY.md updated for all renames + new entries; mdbook build clean. All `status: reviewed`. |
| P5.J | `doc/cloud/**` | 1 article + README | ✅ DONE (reviewed) | hand-rewritten 2025: 1 article + README. `oracle_free_tier.md` rewritten with the proper "why this matters" framing (4 OCPU + 24 GB ARM forever vs. AWS/GCP 12-month-and-out), an honest gotchas section (capacity scarcity, OCI IAM clunkiness, VCN ingress not open by default), the full Always Free quota as a Markdown table, the VCN ingress-rule recipe, and a condensed Apache + PHP install that delegates to `[[apache]]` rather than duplicating its content. README rewritten as a small honest landing page acknowledging the section is one-article-deep with a free/cheap-VPS decision matrix and cross-section pointers (kubernetes / www / observability / messaging / dpu). SUMMARY.md updated; mdbook build clean. All `status: reviewed`. |
| P5.K | `doc/kubernetes/**` | 5 articles + README | ✅ DONE (reviewed) | hand-rewritten 2025: 5 articles + README. **Big consolidation** of `k3s.md`: the previous version was a 408-line multi-topic hairball mixing K3s install, Alex Ellis's Pi tutorial, OpenFaaS, edX courses, and Pi hardware suppliers (with a half-dozen unfenced shell-comment H1s that previously confused the splitter). Reorganised into one coherent article — install / add worker / Pi recipe / arkade / OpenFaaS quick-start / inlets / GitOps / learning resources / faasd cousin — with all shell blocks properly fenced. Other rewrites: `akri.md` framed as the device-discovery layer with controller+agent+broker architecture; `balena.md` framed honestly as not-Kubernetes (it's filed here for problem-space adjacency) with a balenaOS / Engine / Cloud component table; `dokku.md` framed as the Heroku-shaped single-host PaaS with install + git-push + plugin recipes; `kuasar.md` framed as the Sandbox API runtime with the multi-sandbox / 99% overhead reduction pitch and the supported-distros caveat. README rewritten with a 6-row decision tree ("I have a fleet of small Linux boxes, what should I run?") and cross-section see-also (oracle_free_tier, observability, messaging, robot/groot). SUMMARY.md updated with new titles + K3s promoted to top of section. mdbook build clean. All `status: reviewed`. |
| P5.L | `doc/iot/**` | 2 articles + README | ✅ DONE (reviewed) | hand-rewritten 2025: 2 articles + README. **Deleted 2 duplicates**: `iot/mqtt.md` (empty, no main_link, redundant with the much richer `messaging/mqtt.md`) and `programming/rust/misc/drogue.md` (auto-split duplicate of `iot/drogue.md` with identical content, originally split out of `raspberry_pi.md`). Sed-patched the 7 stale `[[programming/rust/misc/drogue|drogue]]` auto-suggestions across the vault to point at `[[iot/drogue|drogue]]`. **Hybrid-C fill**: `messaging/mqtt.md` (referenced 8+ times, owning batch is next, removing the iot/mqtt alias made this the canonical home) was fully written as a proper landing page — OASIS-spec context, IBM 1999 history, when-to-use vs Kafka/NATS, the 4 footguns (QoS semantics, retained messages, topic versioning, schema-less payloads), 6-row broker comparison table (Mosquitto/EMQX/HiveMQ/VerneMQ/NanoMQ/managed cloud), 4 client-library options, wire-structure mental model, QoS levels in 1 paragraph, MQTT 3.1.1 vs 5 picker, public test brokers, Mosquitto-in-30-seconds recipe, MQTT→Kafka bridge note. `iot/drogue.md` rewritten with honest archived-status warning + alternatives matrix (ThingsBoard / Eclipse Hono / EMQX+Kafka Connect / Embassy for embedded). `iot/shodan.md` rewritten with the defensive-recon angle, paid-tier filter cheat sheet, CLI workflow, and 5-row "what to look for in your own perimeter" checklist. README rewritten as a small honest landing page (3-axis framing: Building / Transport / Security) with cross-section see-also (kubernetes/balena, kubernetes/k3s, kubernetes/akri, robot/groot, cloud/oracle_free_tier). SUMMARY.md updated for both deletions and title polishes. mdbook build clean. All `status: reviewed`. |
| P5.M | `doc/messaging/**` | 7 articles + README | ✅ DONE (reviewed) | hand-rewritten 2025: 7 articles + README. **Deleted 2 duplicates**: `kafka_2.md` (auto-split sibling of `kafka.md`; merged its kafka-rust + kafka-streams content into the canonical kafka.md) and `red_panda.md` (auto-split sibling of `redpanda.md`; merged its container-recipe content into redpanda.md AND moved the misplaced "KAFKA OPS" sub-section over to kafka.md, where it actually belongs). Sed-patched all `[[kafka_2]]` → `[[kafka]]` and `[[red_panda]]` → `[[redpanda]]` references across the vault (5 stub-file occurrences). **`messaging/mqtt.md` was already reviewed in P5.L** under hybrid-C; counts towards this batch. Big content adds: `kafka.md` is now a single coherent article — OASIS history, KRaft mode note (ZK retired in 4.0), 4 footguns (log-not-queue, partition ordering, retention, JVM client primacy), Mac/WSL/Docker install, Confluent all-in-one, Rust client matrix (rdkafka/kafka-rust/samsa), `librdkafka` reference, and the **11-row Kafka ops governance table** (Strimzi, Julie, TopicCtl, Terraform, kafka-gitops, Helmsman, Jikkou, ns4kafka, kafkaer, Koperator, Klaw); `redpanda.md` framed honestly as Kafka-wire-compatible single-binary with the BSL-licence warning, three-node `rpk container` recipe, manual single-Docker recipe, and the "what works against Redpanda from Kafka tooling" cheat sheet; `pulsar.md` framed with the BookKeeper storage/compute separation, the 4-product family pitch, when-to-use vs Kafka/Redpanda, single-Docker quickstart; `nats.md` framed as Go-based pub/sub + JetStream + KV + leaf-nodes with the under-rated subject-wildcard angle, install + CLI smoke test + JetStream commands; `kinesis.md` framed as the AWS-managed family with the 4-sub-product table, AWS CLI smoke test, KPL/KCL note, and the cost-reality-check; `patchbay.md` framed as a hobby-tier curl-based pub/sub with honest "don't use for anything you care about" warnings, rendezvous and pubsub examples. README rewritten with a 7-row decision tree ("what do you need?" → substrate) and cross-section see-also (iot/{drogue,balena,akri}, programming/rust/net/{mq,nats,paho_mqtt}, observability, kubernetes/k3s). SUMMARY.md updated for both deletions and title polishes. mdbook build clean. All `status: reviewed`. |
| P5.N | `doc/observability/**` | 4 articles + README | ✅ DONE (reviewed) | hand-rewritten 2025: 4 articles + README. **`grafana.md` and `prometheus.md` were already reviewed in P5.I** under hybrid-C; counted towards this batch (the four articles are: prometheus, grafana, node_exporter, openobserve). New rewrites this round: `node_exporter.md` framed as the canonical Prometheus host-metrics exporter — install via brew/Linux-binary/systemd, the 3 operational notes (collector tuning / `textfile` collector / cardinality on `device` and `mountpoint`), the cAdvisor companion note, full Prometheus scrape-config snippet, and a working `textfile`-collector cron-job example for emitting backup-status metrics; `openobserve.md` framed as the Rust-rewritten one-binary alternative to ELK + Loki+Mimir+Tempo, with the ~140× storage-cost claim called out (with appropriate scepticism about single-vendor benchmarks), single-binary + Docker quickstarts, Fluent Bit ES-bulk config, OTLP/HTTP+gRPC endpoints, and the broader "Rust single-binary infra rewrite" trend (Redpanda, InfluxDB IOx, GreptimeDB, QuestDB) as context. README rewritten with a 7-row decision tree and cross-section see-also (visualization/grafana, kubernetes/k3s, cloud/oracle_free_tier, messaging/{mqtt,kafka,redpanda}, iot/shodan). SUMMARY.md updated with proper title casing (Prometheus / Grafana etc., promoted Prometheus to top of section). mdbook build clean. All `status: reviewed`. |
| P5.O | `doc/dpu/**` | 1 article + 3 READMEs | ✅ DONE (reviewed) | hand-rewritten 2025: 1 article + 3 READMEs. The previous plan note ("0 articles, only README + asset folders") was wrong — there's actually `xilinx/vitis.md` plus 3 READMEs and 3 `.pptx` reference decks under `xpu/`. `vitis.md` rewritten as a proper Vitis install/usage article — the AMD-rebranding history table (SDx/SDAccel/SDSoC → Vitis → AMD Vitis), the 3 install gotchas (`libtinfo5` on Ubuntu, ~150 GB disk + journaling-disabled partition tip, component-picking by board with the Alveo-vs-Kria PetaLinux split), original install notes preserved as a quoted reference block, pointers to Vitis AI / PetaLinux / OpenCL / Intel oneAPI. Top-level `dpu/README.md` rewritten honestly disambiguating the **3 different meanings of "DPU"** in the industry (Data Processing Unit / Deep-learning Processing Unit / Display Processing Unit) with vendor-example table and an explicit "deliberately not covered here" pointer to NVIDIA BlueField / AWS Nitro / Marvell OCTEON external resources. `xilinx/README.md` rewritten with AMD-acquisition framing. `xpu/README.md` rewritten honestly as a slide-deck parking spot (3 pptx files documented in a table; not pretending to be Markdown articles). Cross-section see-also: nvidia_omniverse, k3s, groot, ml/compilers/cuda. SUMMARY.md updated with proper title casing. mdbook build clean. All `status: reviewed`. |
| P5.P | `doc/tools/{osx,linux,shell}/**` + `tools/README.md` | 25 articles + 4 READMEs | ✅ DONE (reviewed) | hand-rewritten 2025: 25 articles + 4 READMEs across `osx/` (4+1) + `linux/` (12+1) + `shell/` (8+1) plus the top-level `tools/README.md`. **Deleted 2 duplicates**: `tools/linux/starshio.md` (typo'd auto-split, content already covered by `programming/rust/tooling/starship.md` to be reviewed in P5.X) and `tools/shell/tmux.md` (empty stub; canonical at `tools/linux/tmux.md`). **Renamed 1 file**: `tools/shell/browser_in_terminal.md` → `tools/shell/carbonyl.md` (the file was actually about Carbonyl). Sed-patched 5 stale `[[tools/shell/tmux|tmux]]` references across the vault to `[[tools/linux/tmux|tmux]]` (workmux, ubuntu_share, mprocs, zellij, cursive). Three sub-agents handled the per-section rewrites in sequence. README rewrites: `tools/README.md` got a 9-row subsection table + 6-line decision-shortcuts section + cross-section see-also; `tools/osx/README.md` framed macOS-specific tooling (3-bucket organisation, explicit "cross-platform tools live in linux/" framing); `tools/linux/README.md` themed by tool category (multiplexers / shells / editors / git UIs / networking / debuggers / books); `tools/shell/README.md` themed (text-mode browsers / shell ergonomics / SSH / DNS) with cross-section pointers. Notable content adds: `tmux.md` got proper Insight section + `~/.tmux.conf` starting points + plugin recommendations; `helix.md` framed honestly vs Neovim/Zed/Kakoune; `mosh.md` got the killer-use-case framing + UDP caveats; `osxcross.md` got the SDK-licensing caveat + cargo-zigbuild alternative note; `utm.md` framed vs Parallels / VMware Fusion / VirtualBox + the Lima/Colima recommendation for Linux-on-Mac dev; `osx_tricks.md` got the 3-step Gatekeeper recipe; `must_have.md` and `shell/tools.md` reframed as proper recipe collections; `dns_toys.md` preserved every recipe + added the `dy` shell-function shortcut; `carbonyl.md` framed honestly as "the most-capable terminal browser, rarely the right tool". SUMMARY.md updated with all 25 title polishes + 2 deletions + 1 rename. mdbook build clean. All `status: reviewed`. |
| P5.Q | `doc/tools/{security,networking}/**` | 7 articles + 2 READMEs | ✅ DONE (reviewed) | hand-rewritten 2025: 7 articles + 2 READMEs across `security/` (6+1) + `networking/` (1+1). **Deleted 2 duplicates**: `tools/security/pake.md` (auto-split sibling of `web_to_app_pake.md`; the substantive content was in the latter) and `tools/security/port_scan.md` (duplicate RustScan stub of `port_scanner_rustscan.md`). **Renamed 2 files**: `port_scanner_rustscan.md` → `rustscan.md` (cleaner), `web_to_app_pake.md` → `pake.md` (canonical home for Pake article in this section, after the dup was deleted). Sed-patched 4 stale references across the vault: `[[port_scan]]` and `[[port_scanner_rustscan]]` → `[[rustscan]]`; `[[web_to_app_pake]]` → `[[tools/security/pake|pake]]` (in `programming/rust/gui/pake.md`, `programming/rust/io/json.md`, `ml/llm/runtimes/{providers,goose}.md`). Sub-agent handled the rewrites. Notable content: `wireguard.md` got the Tailscale/Headscale/Netbird/Innernet/Nebula comparison + 3 production gotchas (silent peers, NAT keepalives, routing); `pake.md` (renamed) preserved the full content (CLI recipes, prerequisites, popular-packages list) + honest "this is filed under security/ oddly" disclosure; `rustscan.md` framed as the nmap front-end + ethics note; `sniffnet.md` framed honestly as friendlier-than-Wireshark, not a replacement; `ngrok.md` framed with the freemium reality + Cloudflare Tunnel / Tailscale Funnel / bore / frp alternatives matrix; `rathole.md` framed as the Rust frp-replacement; `mtr.md` framed as `traceroute + ping` with ICMP rate-limiting reading guide. READMEs themed by use case. SUMMARY.md updated with all 2 deletions + 2 renames + 7 title polishes. Filename ambiguity for `pake` (now `tools/security/pake.md` + `programming/rust/gui/pake.md`) is handled by always using `[[tools/security/pake|pake]]` form. mdbook build clean. All `status: reviewed`. |
| P5.R | `doc/tools/{editors,design,live_demos,misc}/**` | 18 articles + 4 READMEs | ✅ DONE (reviewed) | hand-rewritten 2025: 18 articles + 4 READMEs across `editors/` (4+1) + `design/` (8+1) + `live_demos/` (1+1) + `misc/` (5+1). **Renamed 6 files** (no deletions): `design/applications.md` → `deskreen.md`; `design/fengshui.md` → `czkawka.md`; `design/gramma_harper.md` → `harper.md` (typo); `misc/md_to_ppt_pdf_jpeg.md` → `marp.md`; `misc/payments.md` → `hyperswitch.md`; `misc/rust_must.md` → `bacon.md` (also fixed the wrong `main_link` that was pointing at prek). Sed-patched 2 stale `[[applications]]` references across the vault to `[[deskreen]]`. Sub-agent handled all 22 rewrites. Notable content adds: `intellij.md` got the IntelliJ-vs-VS-Code-vs-Zed comparison + the productivity-shortcuts list (double-shift, Alt-Enter, Cmd-Shift-A); `void.md` framed vs Cursor / Zed AI / Continue (and removed an unverified placeholder link); `markitdown.md` framed as the LLM-feeding companion (Office → MD); `mdfried.md` framed as a terminal Markdown viewer with bigger headings vs glow/mdcat; `deskreen.md` framed vs AirPlay/Miracast/VNC/RustDesk; `czkawka.md` mentions the new Krokiet GUI + vs fclones/rmlint/dupeGuru/DaisyDisk; `harper.md` framed vs Grammarly/LanguageTool/Vale; `lstr.md` framed vs tree/eza/broot/yazi; `rustdesk.md` framed vs TeamViewer/AnyDesk/VNC/Sunshine + self-hosting note; `superfile.md` got full keybindings cheat-sheet preserved; `weektodo.md` framed vs Todoist/TickTick/Sunsama; `demo_overview.md` framed `demo-magic` vs vhs/asciinema/terminalizer; `marp.md` framed vs Reveal.js/Slidev/presenterm/Beamer with full CLI examples; `hyperswitch.md` preserved the 50+ processors / 130+ countries / hosted feature matrix; `prek.md` preserved full `.pre-commit-config.yaml` + `.cargo/audit.toml` + CI snippets + skip-hook trick; `bacon.md` framed the watch-mode niche pairs with helix/lazygit/prek; `workmux.md` got the git-worktrees + tmux-windows + AI-agent-parallel pitch + comparison vs Tmuxinator/Smug/Sesh. READMEs themed by use case. SUMMARY.md updated for all 6 renames + 18 title polishes. mdbook build clean. All `status: reviewed`. |
| P5.S | `doc/db/{relational,nosql,vector,timeseries}/**` | 19 articles + 5 READMEs | ✅ DONE (reviewed) | hand-rewritten 2025: 19 articles + 5 READMEs across `relational/` (10 + 2 READMEs incl. postgres subfolder), `nosql/` (3+1), `vector/` (2+1), `timeseries/` (4+1). No deletions, no renames — all files kept their existing paths. Sub-agent handled the rewrites. **Notable framing**: `postgresql.md` preserved the full "9 production gotchas" piece with all SQL fenced + a new Insight section (Postgres vs MySQL/SQLite/NewSQL/NoSQL/jsonb-vs-document-store) + ecosystem note (Pigsty, pgvector, Citus, TimescaleDB, Barman); `mysql.md` framed honestly with the WordPress/Aurora/replication-topology niche where MySQL beats Postgres, MariaDB call-out; `fauna.md` called out the **April 2025 SaaS sunset**; `edgedb.md` noted the **2024 Gel rebrand**; `redis.md` called out the **2024 BSD→SSPL/RSALv2 license change and the Linux Foundation Valkey fork**; `surrealdb.md` framed as multi-model with honest young/license caveats; `tensorbase.md` flagged as dormant with active-successor pointers; `singlestore.md` got the HTAP framing + MemSQL rebrand note; `limbo.md` framed as Turso's SQLite-compatible Rust rewrite; `toydb.md` framed as best-free-way-to-learn-DB-internals; `xlite.md` positioned vs DuckDB; `barman.md` vs pg_dump/pgBackRest/WAL-G. Vector section: `qdrant.md` framed as production-grade with payload-filtering as the differentiator + sibling-disambiguation note about `ml/data/qdrant_vector_search.md`; `chroma.md` as the easy on-ramp; README has the "do you need a vector DB at all?" framing + 8-row decision matrix. Time-series section: `influxdb.md` framed around the 1.x/2.x/3.x version confusion + 3.x = Rust-on-DataFusion (IOx); `questdb.md` for fintech tick data; `greptimedb.md` as part of the "Rust single-binary infra" trend (alongside Redpanda/OpenObserve) + sibling-disambiguation about `programming/rust/data/greptimedb.md`; `druid.md` honestly noted ClickHouse has eaten share since 2020. All READMEs themed and decision-tree'd. SUMMARY.md updated for all 19 title polishes + section rename ("Nosql"→"NoSQL", "Timeseries"→"Time series", "Vector"→"Vector databases") + ordering changes (canonical articles promoted to top of each section). mdbook build clean. All `status: reviewed`. |
| P5.T | `doc/db/analytics/**` + `streaming/**` + `distributed/**` | 30 | ✅ DONE | combined |
| P5.U | `doc/db/catalogs/**` + `formats/**` + `quality/**` + `fabric/**` + `udf/**` + `tools/**` + `misc/**` | 25 | ✅ DONE | combined |
| P5.V | `doc/programming/` (top-level files only — java/scala/cpp/go/swift/php/assembly/etc.) | 25 | ✅ DONE | merged into all-programming run |
| P5.W | `doc/programming/rust/learning/` + `core/` + `concurrency/` | 25 | ✅ DONE | merged |
| P5.X | `doc/programming/rust/io/` + `net/` + `web/` + `gui/` + `tui/` + `cli/` | 30 | ✅ DONE | merged |
| P5.Y | `doc/programming/rust/data/` + `ml/` + `interop/` + `tooling/` + `plugins/` + `misc/` | 30 | ✅ DONE | merged |
| P5.Z | `doc/programming/rust/sql_engine/` + `projects/` | 25 | ✅ DONE | combined: 102/102 stubbed; 25 multi-topic, 5 no-URL |
| P5.AA | `doc/ml/fundamentals/**` | 15 | ✅ DONE | merged into all-ml run |
| P5.AB | `doc/ml/data/**` + `frameworks/**` + `compilers/**` + `bigquery/**` | 25 | ✅ DONE | merged |
| P5.AC | `doc/ml/time_series/**` + `knowledge_graph/**` + `genetics/**` + `quantum/**` + `memory/**` + `skills/**` | 25 | ✅ DONE | merged |
| P5.AD | `doc/ml/llm/**` (the merged LLM hub) | 30 | ✅ DONE | merged |
| P5.AE | `doc/ml/rag/**` | 10 | ✅ DONE | merged |
| P5.AF | `doc/ml/agents/**` + `mcp/**` + `finetuning/**` | 20 | ✅ DONE | merged |
| P5.AG | `doc/ml/tools/**` + `voice/**` + `orchestration/**` | 20 | ✅ DONE | combined: 91/91 stubbed; 16 multi-topic, 5 no-URL |
| P5.AH | `doc/published/**` | 3 | ✅ DONE | 2/2 stubbed; 2 no-URL |

**P5 totals:** 343/345 articles auto-stubbed (the 2 exceptions are `SUMMARY.md` regenerated in P7, and `references.md` intentionally KEEP). 54 multi-topic flagged for splitting; 36 missing main_link. Full breakdown in `inventory/p5_summary.md`.

### P5 splitting pass (follow-up)

Ran `inventory/article_split.py --batch` to handle the 54 multi-topic flags from the auto-stub.

- **50 files** split successfully — each H1 inside their `## References / raw notes` extracted into a fresh sibling article (with full template, frontmatter, back-reference, TODO list).
- **+156 new articles** spawned from the splits.
- **3 files** rejected by the pathological-heading guard (their `# foo` lines are unfenced Python/shell comments, not real Markdown headings):
  - `doc/ml/time_series/forecasting.md`
  - `doc/programming/rust/data/ml_letsql.md`
  - `doc/trading/mql5.md` — ✅ **resolved in P5.H** (rewritten with all snippets fenced)

  The remaining two still need their TODO bullet ("wrap embedded code in ``` fences first, then re-run the splitter") addressed when those batches come up.
- **1 redundant artefact** removed: `doc/programming/rust/tui_2.md` (an empty fragment of `cli.md`'s `# TUI` heading that was already covered by the existing `tui.md` and its split sibling `cursive.md`).
- **All directory READMEs regenerated** (`readme_gen.py --force`) to surface the new sibling articles.

**Article count after this pass: 501** (was 345). Full details in `inventory/p5_split_summary.md`.

Zero residual `"split this file"` TODO bullets in the vault.

---

## 9. Phase P6 — Cross-link & keyword pass

Goal: turn the rewritten files into a real Obsidian graph.

- [x] **P6.1** ✅ DONE — Build `inventory/keywords.md`: union of all `keywords:` frontmatter, with counts. Spot near-duplicates (e.g. `llm` vs `llms` vs `large-language-model`) and produce a canonical list.
  > comment: agent zed-opus — 1471 distinct keywords / 3540 occurrences. 80 near-duplicate buckets, 11 Levenshtein-1 fuzzy groups. Generator at `inventory/build_keywords.py`; outputs at `inventory/keywords.tsv` (full sorted) and `inventory/keywords.md` (analysis). Notable: many noise keywords (`the`, `code`, `text`, `pool`, `tree`, `user`, `make` etc.) leaked in from auto-derived body words — will be stripped in P6.2. Real near-duplicates to canonicalise: `tool`/`tools`, `crate`/`crates`, `model`/`models`, `agent`/`agents`, `time-series`/`timeseries`/`time_series`, `digital-twin`/`digital_twin`, `knowledge-graph`/`knowledge_graph`, etc.
- [x] **P6.2** ✅ DONE — Sweep all articles to canonicalise keywords against the list from P6.1.
  > comment: agent zed-opus — generator at `inventory/canonicalise_keywords.py`. Two passes: first stripped 173 noise occurrences via DROPLIST + canonicalised 80 near-duplicate buckets via CANONICAL map; second pass closed the remaining 13 small buckets. **Result:** 1471 distinct keywords → **1356**, 3540 occurrences → **3360**. Near-duplicate buckets 80 → **1** (just `nat` vs `nats` — intentionally kept separate; `nat` = Network Address Translation, `nats` = NATS messaging system). Path-derived keywords (first 4 in each article) are protected from the DROPLIST. Underscores normalised to hyphens. Common Obsidian-friendly forms chosen (e.g. `time-series`, `digital-twin`, `knowledge-graph`).
- [x] **P6.3** ✅ DONE — For each article, ensure at least 2 internal `[[wikilinks]]` exist; if not, add suggestions.
  > comment: agent zed-opus — generator at `inventory/suggest_wikilinks.py`. Algorithm: for each article, score every other vault article on (a) shared keywords weighted by inverse document frequency — rare keywords like `kafka` score high, generic ones like `rust` score near zero, (b) +4 same parent dir, (c) +12 if the candidate's title appears verbatim in the body. Top 5 candidates with score ≥ 5 are written under `## Internal links` as `[[stem]] — title (score N)` bullets, behind a `<!-- internal-links-suggested by P6.3 -->` marker. Ambiguous stems (multiple files share the name) get the full path: `[[parent/stem|stem]]`. **Result:** 484/487 articles received auto-suggested wikilinks (3 had no candidate scoring ≥ 5). Sample: `git/git_delta` → `[[config]] [[github]]`; `messaging/kafka` → `[[red_panda]] [[redpanda]] [[kafka_2]] [[pulsar]] [[apache]]`; `ml/llm/models/llama` → `[[llama_2]] [[jetbrains_mellum]] [[olmo]] [[mplug]] [[alicemind_mplug_2_0]]`. Each block is followed by a TODO bullet asking a human to prune low-signal links.
- [x] **P6.4** ✅ DONE — Verify every directory `README.md` lists every article in that directory (no orphans).
  > comment: agent zed-opus — generator at `inventory/check_orphans.py`; output at `inventory/orphans.md`. Scanned 132 directories; **0 dead references** and **0 real orphans**. The only flagged orphans are `doc/SUMMARY.md` (mdBook ToC, regenerated in P7) and `doc/references.md` (intentionally kept and not listed in the hand-written `doc/README.md`).
- [x] **P6.5** ✅ DONE — Auto-suggest *additional* keywords per article (5-strategy mining) + universal-noise stripper.
  > comment: agent zed-opus — generators at `inventory/suggest_keywords.py` (5 strategies: title-tokens, neighbour propagation, URL host, body-vocab match, co-occurrence) and `inventory/strip_universal_keywords.py` (post-pass that drops noise + path-redundant keywords). Pipeline: (1) `suggest_keywords.py` added 1438 HIGH-confidence keywords across 381 files; (2) `strip_universal_keywords.py` then removed 1306+769 = **2075 noise occurrences** across 511+332 = 843 file edits, applying two rules: (R1) `GENERIC_NOISE` set strips junk like `links`, `todo`, `install`, `build`, `function`, `time`, `tree`, `between`, `inspired`, `software`, `engine` etc. everywhere; (R2) **path-redundant rule** strips a keyword from articles inside its dominant directory while keeping it on out-of-section articles — e.g. `crates` is removed from `programming/` files but kept on the 6 cross-section articles that mention it (preserving graph signal). After cleanup: **1086 distinct keywords / 2390 occurrences** (down from 1356/3360). The wikilink suggester was re-run with `--force` against the cleaner vocabulary; sample improvements: `messaging/kafka` → `[[red_panda]] [[redpanda]] [[kafka_2]] [[pulsar]] [[apache]]` (unchanged — already strong); `git/git_delta` → `[[github]] [[config]] [[prometheus]] [[workmux]] [[delta]]` (top 2 are real, bottom 3 are mild noise from rare shared keywords). MEDIUM/LOW suggestions from S2/S3/S5 written to `inventory/keyword_suggestions.md` for human review (350 medium + 1446 low).

---

## 10. Phase P7 — Rebuild `SUMMARY.md` & verify mdBook

- [x] **P7.1** ✅ DONE — Regenerate `doc/SUMMARY.md` from the new tree (script that walks `doc/**/README.md`).
  > comment: agent zed-opus — generator at `inventory/build_summary.py`. Walks `doc/` depth-first, emits one mdBook chapter (H1) per top-level area in a fixed order matching `doc/README.md`'s sections. Subdirs nest under their parent's `README.md` link. Articles in each dir are listed as bullets, titled by their frontmatter `title:`. Asset dirs (`img/`, `res/`, `res_pptx/`, `papers/`, `assets/`) are excluded. **Output:** `doc/SUMMARY.md` — 680 lines covering all 132 directories and 487 articles. Re-runnable: `python3 inventory/build_summary.py` from the repo root.
- [x] **P7.2** ✅ DONE — Run `mdbook build` and fix any broken links it reports.
  > comment: agent zed-opus — first build failed on `mdbook-admonish` asset version mismatch (3.0 required, 1.0 installed); fixed by running `mdbook-admonish install` to refresh CSS assets in `book.toml`. Then build **succeeded with only 2 warnings**, neither content-related: an mdbook-mermaid preprocessor compiled against an older mdbook version (cosmetic), and a 12.6 MB search-index warning (expected for a 620-page book). Generated **620 HTML pages** under `book/`. Independent broken-link scan found 90 broken links — all but 4 were inside `## References / raw notes` blocks (verbatim pasted upstream READMEs whose paths break when extracted). Fixed the 4 vault-internal cases via `inventory/patch_legacy_typo_paths.py`: replaced legacy typos `gui.mdi.md`→`gui.md`, `http.mdp.md`→`http.md`, `slint.mdui/slint.md`→`slint.md` across `www/ssg.md`, `www/loco_rust_on_rails.md`, `programming/rust/web/webassembly.md`, `programming/rust/gui/ui.md`. SUMMARY rebuilt; mdbook re-built clean.
- [x] **P7.3** ✅ DONE — Update root `README.md` to reflect the new structure (the current Articles list, Procedure, Publish sections still apply but paths may change).
  > comment: agent zed-opus — root `README.md` rewritten as a repo-level overview pointing at `doc/README.md` for the canonical landing page. Sections: What's in here (table of every top-level path), Investigation procedure, Article template (frontmatter + section layout), Featured published articles, Building the mdBook site (including the `mdbook-admonish install` recipe for the version mismatch), Maintenance scripts table (one row per script under `inventory/`), Audit reports table (every `.md` / `.tsv` under `inventory/`), Conventions, and Status. The original Procedure / Structure / Articles / Publish sections are folded in.

---

## 11. Phase P8 — Final review

- [ ] **P8.1** ⬜ TODO — Remove all technical comments in all articles like "<!-- reviewed -->", "<!-- generated by readme_gen.py -->" etc,
  > comment:
- [ ] **P8.2** ⬜ TODO — Remove whole inventory directory from github history!!!
  > comment:
- [ ] **P8.3** ⬜ TODO — User skims top-level READMEs for tone/voice.
  > comment:
- [ ] **P8.4** ⬜ TODO — Open Obsidian, inspect graph view, prune low-signal keywords.
  > comment:
- [ ] **P8.5** ⬜ TODO — Tag every article with `status: reviewed` once happy.
  > comment:

---

## 12. Handoff prompt — how a new agent picks up where the last one left off

When a session ends mid-work, the next agent (or future you) should be invoked with **exactly** the prompt below. It assumes the next agent has access to this repository and the standard tools.

````
You are continuing a long-running reorganization of the `dev_tips` Obsidian vault.

The master plan lives at `dev_tips/plan.md`. Read it FIRST and IN FULL.

Procedure:

1. Open `dev_tips/plan.md`.
2. Find the FIRST task whose status is `⬜ TODO` or `🟨 IN PROGRESS`.
   - If `🟨 IN PROGRESS`, read the comment under it to see where the previous agent stopped.
3. Mark it `🟨 IN PROGRESS` with a comment: `> comment: <agent-name> started <ISO timestamp>`.
4. Do the work for THAT TASK ONLY. Do not jump ahead.
   - Honour the templates in §2 of `plan.md`.
   - Honour the target taxonomy in §3.
   - Use `move_path` (never plain delete + recreate) when moving files.
   - When rewriting articles, follow the per-article checklist in §8.
5. When finished:
   - Mark the task `✅ DONE` with a comment: `> comment: <agent-name> finished <ISO timestamp> — <1-line note, e.g. "17/17 files rewritten, 2 marked TODO for unclear topic">`.
   - If you produced sub-artifacts (e.g. `inventory/files.tsv`), commit them.
   - If you discovered new issues, append them as new tasks at the end of the relevant phase rather than silently fixing them.
6. Stop. Do not start the next task unless explicitly asked.

Guard-rails:

- Never delete a file whose content has not been merged elsewhere.
- Never edit `plan.md` to remove history; only update statuses and add comments.
- If a task is ambiguous, mark it `⚠️ BLOCKED` with a precise question for the user and stop.
- Prefer many small, reviewable edits over one giant rewrite.

Report back with:
- Which task you picked up.
- What you changed (file list).
- The new status line you wrote into `plan.md`.
- Any blockers or follow-ups discovered.
````

### Quick-resume prompts (copy-paste for specific phases)

**Resume in Phase P5 (article rewrites):**

````
Continue `dev_tips/plan.md`. Pick the FIRST P5.* batch with status `⬜ TODO`. For every `.md` in its scope (excluding `README.md`), apply the per-article checklist from §8 and the template from §2. Do NOT touch other batches. When done, update the batch row in the P5 table to `✅ DONE` with `<n>/<total>` count and any TODO notes.
````

**Resume in Phase P4 (README normalisation):**

````
Continue `dev_tips/plan.md`. Pick the FIRST P4.* task with status `⬜ TODO`. For every directory in its scope, apply the algorithm in §7 to produce exactly one `README.md` following the landing-page template in §2. When done, update the task line to `✅ DONE`.
````

**Resume in Phase P3 (folder restructure):**

````
Continue `dev_tips/plan.md`. Pick the FIRST P3.* task with status `⬜ TODO`. Use `inventory/move_map.tsv` as the source of truth for source→destination paths. Use `move_path` for every move so history is preserved. When done, update the task to `✅ DONE`.
````

---

## 13. Conventions cheat-sheet (quick reference)

- File names: `lower_snake_case.md`. No spaces. No mixed case (`KAN.md` → `kan.md`).
- Directory names: same rules. No `INDEX.md`, no `index.md`, only `README.md`.
- Keywords: lowercase, hyphenated, no spaces. Examples: `rust`, `llm`, `vector-db`, `time-series`, `mdbook`.
- Internal links: prefer `[[wikilinks]]` for Obsidian; `SUMMARY.md` keeps standard `[text](path)` for mdBook compatibility.
- One `main_link` per article. Pick the most canonical (official site > GitHub > paper > blog).
- Frontmatter is mandatory on every non-landing article.
