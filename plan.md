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
| P6 | Cross-link & keyword pass (graph polish) | agent | 🟨 IN PROGRESS |
| P7 | Rebuild `SUMMARY.md` & verify `mdbook build` | agent | ⬜ TODO |
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
│   ├── analytics/                 # databend, datafusion, gluten, tachyons_distributed (typo fixed), tensor_query (space fixed)
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
| `doc/db/tachyons_distriibuted/` | `doc/data/distributed/tachyons_distributed/` |
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
  > comment: agent zed-opus — 30 MOVE + 3 MERGE applied via `apply_move_map.py --dst-prefix doc/db/`. Asset folders (PDFs/PPTX/PNG/excalidraw) for ballista_distributed, data_curation, data_fabric, databend, datafusion, delta_live, federated*, gluten, long_running_query, nimble_file_format, scheduling/{google,microsoft,vldb}, streaming_query_approximation/{img,res}, tachyons_distributed, tensor_query, unity_hive_catalog all relocated under their new taxonomic homes. UDF case-insensitive duplicate handled. Empty source dirs removed. New tree: 12 top-level db sub-categories.
- [x] **P3.4** ✅ DONE — Move everything destined for `doc/ml/` (the big LLM/RAG/agents merge — see §3).
  > comment: agent zed-opus — 62 MOVE + 25 MERGE + 1 RENAME applied. ai/, timeseires/, LLM/, RAG/, RAG_PDF/, rag_query_summarize/, KAN/, ML/, ML_auto_indexing_DBs/, CUDA/, ai/ai_data_orchestration/, no deep learning needed/, zero_shot/, ollama/, agents/, mcp/ all reshaped. LLM hub recovered after a case-insensitive FS hiccup that nested everything inside `papers/` (manually re-flattened). Final tree: 19 top-level ml sub-categories, 106 .md files.
- [x] **P3.5** ✅ DONE — Rename / tidy individual top-level dirs that stay top-level: `WWW→www`, `messagging→messaging`, `DPU→dpu`. (No `infra/` umbrella per user decision.)
  > comment: agent zed-opus — 19+9+2 MOVEs across www/messaging/dpu. Hit a case-insensitive APFS bug where same-inode src/dst caused content loss; fixed `apply_move_map.py` to detect `os.path.samefile()` and do an in-place case-rename via temp name. 11 lost files (WWW articles, mcp4DB.md, vitis.md, external_udfs.md) recovered from `git checkout HEAD --` and re-applied. Final tree: `doc/www/`, `doc/messaging/`, `doc/dpu/` all lowercase; old uppercase aliases gone (HFS/APFS quirk — listings may still echo both cases but inodes are correct).
- [x] **P3.6** ✅ DONE — Move everything destined for `doc/tools/` (regroup security/editors/design/networking).
  > comment: agent zed-opus — 24 MOVE + 4 MERGE applied. New tree: `tools/{design,editors,linux,live_demos,misc,networking,osx,security,shell}/`. Deleted 5 exact-duplicate `<dir>/<dir>.md` files (`tools/tools.md`, `observability/observability.md`, `robot/robot.md`, `iot/iot.md`, `kubernetes/kubernetes.md`, `trading/trading.md`) that exactly matched their `README.md`. 12 other `<dir>/<dir>.md` files have *different* content and are deferred to P4 for proper README merge.
- [x] **P3.7** ✅ DONE — Fix remaining file-name typos from §3 (e.g. `sinffnet→sniffnet`, `kmenas→kmeans`, `gui.mdi.md→gui.md`, `http.mdp.md→http.md`).
  > comment: agent zed-opus — ran the move map for all remaining destination prefixes (visualization, published, cloud, iot, git, observability, digital_twin, funny, games, trading, robot, demo, kubernetes, messaging). Final scan: `find doc -name '*.md' | grep [A-Z] | grep -v README` returns only `SUMMARY.md` (intentional). All article filenames are now lowercase + underscore-separated. Typos fixed in-place by the move map: `sinffnet→sniffnet`, `kmenas→kmeans`, `tachyons_distriibuted→tachyons_distributed`, `unity_hive_catalogur→unity_hive_catalog`, `tensor query→tensor_query`, `agants→agents`, `anmell..md→anmell.md`, `INREX→README`. (Note: `gui.mdi.md`/`http.mdp.md`/`slint.mdui` were never real files — they were only typo paths in `SUMMARY.md`; SUMMARY will be regenerated in P7.)
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

### Per-article checklist (apply to every file)

1. Read the existing file in full.
2. Identify the *thing* it's about. If unclear, leave a TODO comment and continue.
3. If there are multiple topics in one file - split it to topic specific!.
4. Find the canonical `main_link` (project homepage / repo / paper).
5. Write `summary` (2-5 sentences).
6. Write `insight` ("why care, when to use").
7. Add 3-8 `keywords` (lowercase, hyphenated; reuse existing tags where possible – see `inventory/keywords.md`).
8. Find 3-5 `similar/related topics` (other articles in the vault first, then external).
9. Add `internal links` as Obsidian wikilinks `[[...]]` to related articles in the vault.
10. Move all original raw content (links, code blocks, install steps) under `## References / raw notes` so nothing is lost.
11. Add YAML frontmatter (`title`, `main_link`, `keywords`, `status: draft`).
12. Add TODO section if there are still obvious missings, or suggestion what oculd be researched more/.
13. Save file.
14. Update the per-batch status table below with `✅` and a note (e.g. `✅ 17/17 done, 2 marked TODO for unclear topic`).

### Batches (one row per batch — update the status as you go)

| Batch | Scope | File count (approx) | Status | Comment |
|-------|-------|--------------------|--------|---------|
| P5.A | `doc/git/**` | 3 | ✅ DONE | 3/3 stubbed |
| P5.B | `doc/funny/**` | 3 | ✅ DONE | 2/2 stubbed; 1 multi-topic (`brainfuck.md`) |
| P5.C | `doc/games/**` | 5 | ✅ DONE | 4/4 stubbed |
| P5.D | `doc/robot/**` | 2 | ✅ DONE | 1/1 stubbed |
| P5.E | `doc/demo/**` | 1 | ✅ DONE | 1/1 stubbed |
| P5.F | `doc/digital_twin/**` | 10 | ✅ DONE | 9/9 stubbed; 4 no-URL (incl. resurrected `scilab`/`simulink` placeholders) |
| P5.G | `doc/www/**` | 10 | ✅ DONE | 9/9 stubbed; 1 multi-topic, 2 no-URL |
| P5.H | `doc/trading/**` | 9 | ✅ DONE | 7/7 stubbed; 2 multi-topic |
| P5.I | `doc/visualization/**` | 20 | ✅ DONE | 16/16 stubbed; 2 no-URL |
| P5.J | `doc/cloud/**` | 3 | ✅ DONE | 1/1 stubbed |
| P5.K | `doc/kubernetes/**` | 7 | ✅ DONE | 5/5 stubbed; 1 multi-topic (`k3s.md`) |
| P5.L | `doc/iot/**` | 5 | ✅ DONE | 3/3 stubbed; 1 no-URL (`mqtt.md`) |
| P5.M | `doc/messaging/**` | 10 | ✅ DONE | 7/7 stubbed; 1 multi-topic, 2 no-URL |
| P5.N | `doc/observability/**` | 6 | ✅ DONE | 4/4 stubbed; 3 no-URL |
| P5.O | `doc/dpu/**` | 5 | ✅ DONE | 0 articles (only README + asset folders) |
| P5.P | `doc/tools/osx/**` + `linux/**` + `shell/**` | 20 | ✅ DONE | merged into the all-tools run |
| P5.Q | `doc/tools/security/**` + `networking/**` | 10 | ✅ DONE | merged into the all-tools run |
| P5.R | `doc/tools/editors/**` + `design/**` + `live_demos/**` + `misc/**` | 25 | ✅ DONE | combined: 45/45 stubbed; 7 multi-topic, 8 no-URL |
| P5.S | `doc/db/relational/**` + `nosql/**` + `vector/**` + `timeseries/**` | 25 | ✅ DONE | combined: 30/30 stubbed; 2 no-URL (`mysql.md`, `hbase.md`) |
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
  - `doc/trading/mql5.md`

  Their TODO bullet was specialised to "wrap embedded code in ``` fences first, then re-run the splitter".
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
- [ ] **P6.3** 🟨 IN PROGRESS — For each article, ensure at least 2 internal `[[wikilinks]]` exist; if not, add suggestions.
  > comment: agent zed-opus started.
- [ ] **P6.4** ⬜ TODO — Verify every directory `README.md` lists every article in that directory (no orphans).
  > comment:

---

## 10. Phase P7 — Rebuild `SUMMARY.md` & verify mdBook

- [ ] **P7.1** ⬜ TODO — Regenerate `doc/SUMMARY.md` from the new tree (script that walks `doc/**/README.md`).
  > comment:
- [ ] **P7.2** ⬜ TODO — Run `mdbook build` and fix any broken links it reports.
  > comment:
- [ ] **P7.3** ⬜ TODO — Update root `README.md` to reflect the new structure (the current Articles list, Procedure, Publish sections still apply but paths may change).
  > comment:

---

## 11. Phase P8 — Final review

- [ ] **P8.1** ⬜ TODO — User skims top-level READMEs for tone/voice.
  > comment:
- [ ] **P8.2** ⬜ TODO — Open Obsidian, inspect graph view, prune low-signal keywords.
  > comment:
- [ ] **P8.3** ⬜ TODO — Tag every article with `status: reviewed` once happy.
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
