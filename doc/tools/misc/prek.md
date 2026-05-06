---
title: "prek — fast Rust-based pre-commit replacement"
main_link: https://github.com/j178/prek
keywords: [prek, pre-commit, hooks, rust, git, ci, lefthook, husky]
status: reviewed
---

# prek — fast Rust-based pre-commit replacement

**Main link:** <https://github.com/j178/prek>

Site: <https://prek.j178.dev/>

## Summary

`prek` is a single-binary, Rust-based, **drop-in replacement for the `pre-commit` framework** (`pre-commit.com`). It reads the same `.pre-commit-config.yaml`, runs the same hooks, and supports the same commands — but it's ~10× faster, uses ~⅓ the disk space, has zero Python/runtime dependency, ships shell completions, and integrates with `uv` for managing Python tool environments. Author: Jianqiu (j178).

## Insight

The original `pre-commit` is _the_ ubiquitous Git-hook framework, but it has rough edges: slow on large monorepos, sensitive to Python version drift, and weighty on disk because every hook gets its own venv. `prek` is what `pre-commit` should have been by 2024.

Reach for it when:

- Your `pre-commit` runs are slow enough that engineers `--no-verify` past them.
- You're in a Rust/Go/non-Python repo and resent installing Python just for hooks.
- You want one binary in CI without `pip install pre-commit` and a Python container.

It's a **drop-in** — same config, same hook IDs, same commands (`prek install`, `prek run --all-files`). Migrate by `s/pre-commit/prek/g` in your scripts and CI.

Comparisons:

- vs **`pre-commit`** — same UX, much faster, no Python required.
- vs **`lefthook`** — `lefthook` (Evil Martians) is Go, also fast, but uses its own YAML config (not compatible with `pre-commit-hooks` ecosystem). Pick `prek` if you already have a `.pre-commit-config.yaml`; pick `lefthook` if you're starting fresh and prefer its layout.
- vs **`husky`** — JS-only ecosystem; `prek` plays better in polyglot repos.

Skip-hook trick (works the same for `pre-commit` and `prek`):

```bash
git commit --no-verify -m "WIP"
```

## Install

```bash
# Standalone installer (recommended)
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.2.0-alpha.3/prek-installer.sh | sh

# Or via pip / uv
pip install prek
uv tool install prek

# Or via Homebrew
brew install prek
```

## Usage

Replace `pre-commit` with `prek`:

```bash
prek install
prek install --hook-type pre-push
prek run --all-files
prek run rustfmt cargo-deny
prek run --directory backend
prek run --last-commit
prek autoupdate
```

## Similar / related topics

- [`pre-commit`](https://pre-commit.com/) — the Python framework `prek` is compatible with.
- [`lefthook`](https://github.com/evilmartians/lefthook) — Go alternative with its own config.
- [`husky`](https://typicode.github.io/husky/) — JS-only Git hooks helper.
- [[bacon]] — pair: `prek` enforces hooks at commit time, `bacon` runs `cargo check` continuously while you code.
- [[lazygit]] — Git TUI that shows hook output in-line.

## Internal links

<!-- reviewed -->

- [[bacon]] — continuous Rust feedback loop alongside `prek`'s gate at commit time.
- [[workmux]] — multi-worktree workflows where `prek` keeps each branch lint-clean.
- [[lazygit]] — UI for the Git side.

## Keywords

`#prek` `#pre-commit` `#hooks` `#rust` `#git` `#ci` `#lefthook` `#husky` `#misc` `#tools`

## References / raw notes

> ## !!! IMPORTANT !!!
>
> To skip hooks for a specific commit:
>
> ```bash
> git commit --no-verify -m "commit message"
> ```

### Why prek over pre-commit

- **Performance**: 10× faster execution and ⅓ the disk space.
- **No dependencies**: single binary, no Python or runtime requirement.
- **Better UX**: built-in monorepo support, improved commands, shell completions.
- **Compatibility**: drop-in replacement that works with existing `.pre-commit-config.yaml`.
- **Modern tooling**: integration with `uv` for Python environments and improved toolchain management.

### Typical Rust prerequisites you'll lint against

1. **prek** (recommended) or **pre-commit**: `pip install pre-commit`
2. **Rust nightly toolchain**: `rustup toolchain install nightly`
3. **cargo-deny**: `cargo install cargo-deny`
4. **cargo-audit**: `cargo install cargo-audit`

### Hook recipe used in production

A representative `.pre-commit-config.yaml` using both upstream hooks and `local` Rust hooks (the original notes were tuned for a Rust backend in a polyglot monorepo with an excluded `frontend/`):

```yaml
# Pre-commit hooks for the entire project
# See https://pre-commit.com/ for more information

repos:
  # Basic file checks (run on all files)
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.6.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-toml
      - id: check-json
      - id: check-added-large-files
      - id: check-merge-conflict
      - id: check-case-conflict
      - id: debug-statements
      - id: mixed-line-ending
        args: ['--fix=lf']

  # Rust formatting with nightly toolchain (backend only)
  - repo: local
    hooks:
      - id: rustfmt
        name: rustfmt
        entry: bash -c 'cd backend && cargo +nightly fmt --all -- --check'
        language: system
        files: ^backend/.*\.rs$
        pass_filenames: false
        always_run: true
        stages: [pre-commit]

  # Rust clippy linting (backend only)
  - repo: local
    hooks:
      - id: rust-clippy
        name: rust-clippy
        entry: bash -c 'cd backend && cargo clippy --all-targets --all-features -- -D warnings'
        language: system
        files: ^backend/.*\.rs$
        pass_filenames: false
        always_run: true
        stages: [pre-commit]

  # cargo-deny — security and license checks (backend only)
  - repo: local
    hooks:
      - id: cargo-deny
        name: cargo-deny
        entry: bash -c 'cd backend && cargo deny check'
        language: system
        files: ^backend/.*\.(toml|lock)$
        pass_filenames: false
        always_run: true
        stages: [pre-commit]

  # cargo-audit — security vulnerabilities (backend only)
  - repo: local
    hooks:
      - id: cargo-audit
        name: cargo-audit
        entry: bash -c 'cd backend && cargo audit'
        language: system
        files: ^backend/.*\.(toml|lock)$
        pass_filenames: false
        always_run: true
        stages: [pre-commit]

  # cargo-check — ensure code compiles (backend only)
  - repo: local
    hooks:
      - id: cargo-check
        name: cargo-check
        entry: bash -c 'cd backend && cargo check --all-targets --all-features'
        language: system
        files: ^backend/.*\.rs$
        pass_filenames: false
        always_run: true
        stages: [pre-commit]

default_install_hook_types: [pre-commit, pre-push]
fail_fast: false
minimum_pre_commit_version: "3.0.0"

files: \.(rs|toml|lock|yaml|yml|json)$
exclude: |
  (?x)^(
    backend/target/.*|
    \.git/.*|
    \.cargo/.*|
    backend/Cargo\.lock$|
    frontend/.*|
    docs/.*|
    environments/.*|
    tools/.*
  )$
```

### Companion `.cargo/audit.toml`

```toml
# May live in `~/.cargo/audit.toml` or in the project root `.cargo/audit.toml`.

[advisories]
ignore = [
    # ignore "Marvin Attack: potential key recovery through timing sidechannels"
    # — present on sqlx-mysql 0.8.3 (transitive dependency we don't use directly).
    "RUSTSEC-2023-0071",
]
informational_warnings = ["unmaintained"]
# severity_threshold = "low"

# Output configuration
[output]
format = "terminal"  # "terminal" or "json"
quiet = false
show_tree = true

[yanked]
enabled = true
update_index = true
```

### CI integration

```yaml
- name: Run prek hooks
  run: |
    curl --proto '=https' --tlsv1.2 -LsSf \
      https://github.com/j178/prek/releases/download/v0.2.0-alpha.3/prek-installer.sh | sh
    prek run --all-files
```

Or the legacy pre-commit equivalent:

```yaml
- name: Run pre-commit hooks
  run: |
    pip install pre-commit
    pre-commit run --all-files
```

### Adding a new hook

```bash
# With prek (recommended)
prek install
prek run --all-files

# With pre-commit
pre-commit install
pre-commit run --all-files
```

### Troubleshooting

1. **`cargo +nightly fmt` not found** → install nightly toolchain.
2. **`cargo-deny` not found** → `cargo install cargo-deny`.
3. **`cargo-audit` not found** → `cargo install cargo-audit`.

### Updating hook versions

```bash
prek autoupdate
# or
pre-commit autoupdate
```
