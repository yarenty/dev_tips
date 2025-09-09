# Bacon

https://dystroy.org/bacon/


bacon is a background code checker.

It's designed for minimal interaction so that you can just let it run, alongside your editor, and be notified of warnings, errors, or test failures in your Rust code.

It conveys the information you need even in a small terminal so that you can keep more screen estate for your other tasks.

It shows you errors before warnings, and the first errors before the last ones, so you don't have to scroll up to find what's relevant.

![](https://dystroy.org/bacon/img/vi-and-bacon.png)




# Prek


https://prek.j178.dev/

## !!! IMPORTANT !!!

To skip hooks for a specific commit:
```bash
git commit --no-verify -m "commit message"
```


## Recommended Tool: prek

**Use [prek](https://github.com/j178/prek) instead of the original pre-commit tool.**

Rust-based **prek** offers significant advantages over the original pre-commit tool:

- **Performance**: 10x faster execution and 1/3 the disk space usage
- **No Dependencies**: Single binary with no Python or runtime requirements
- **Better UX**: Built-in monorepo support, improved commands, and shell completions
- **Compatibility**: Drop-in replacement that works with existing configurations
- **Modern Tooling**: Integration with uv for Python environments and improved toolchain management

For more details, visit the [prek GitHub repository](https://github.com/j178/prek).

### Installation

Install prek using one of these methods:

```bash
# Standalone installer (recommended)
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.2.0-alpha.3/prek-installer.sh | sh

# Or via pip/uv
pip install prek
# or
uv tool install prek

# Or via Homebrew
brew install prek
```

### Usage with prek

Simply replace `pre-commit` with `prek` in all commands:

```bash
# Install hooks
prek install

# Run all hooks
prek run --all-files

# Run specific hooks
prek run rustfmt cargo-deny
```

## Prerequisites

Before using pre-commit hooks, ensure you have the following installed:

1. **prek** (recommended) or **pre-commit**: Install with `pip install pre-commit`
2. **Rust nightly toolchain**: Install with `rustup toolchain install nightly`
3. **cargo-deny**: Install with `cargo install cargo-deny`
4. **cargo-audit**: Install with `cargo install cargo-audit`

## Installation

### Using prek (Recommended)

1. Install prek hooks from the project root:
   ```bash
   prek install
   ```

2. Install prek hooks for pre-push as well:
   ```bash
   prek install --hook-type pre-push
   ```

### Using pre-commit (Alternative)

1. Install pre-commit hooks from the project root:
   ```bash
   pre-commit install
   ```

2. Install pre-commit hooks for pre-push as well:
   ```bash
   pre-commit install --hook-type pre-push
   ```

## Directory-Specific Behavior

The pre-commit configuration is designed to run different checks based on the file location:

- **All files**: Basic file quality checks (whitespace, formatting, etc.)
- **Backend directory only**: Rust-specific checks (rustfmt, clippy, cargo-deny, etc.)
- **Excluded directories**: `frontend/`, `docs/`, `environments/`, `tools/` are excluded from most checks

## Hooks Included

### File Quality Checks (All Files)
- **trailing-whitespace**: Removes trailing whitespace
- **end-of-file-fixer**: Ensures files end with newline
- **check-yaml**: Validates YAML files
- **check-toml**: Validates TOML files
- **check-json**: Validates JSON files
- **check-added-large-files**: Prevents large files from being committed
- **check-merge-conflict**: Detects merge conflict markers
- **check-case-conflict**: Detects case conflicts in filenames
- **debug-statements**: Detects debug statements
- **mixed-line-ending**: Ensures consistent line endings (LF)

### Rust-Specific Hooks (Backend Directory Only)
- **rustfmt**: Formats Rust code using `cargo +nightly fmt --all` // temporary off - too many fixes to apply
- **rust-clippy**: Lints Rust code using `cargo clippy` with strict warnings
- **cargo-deny**: Checks for security vulnerabilities and license issues
- **cargo-audit**: Audits dependencies for known security issues
- **cargo-check**: Ensures code compiles with `cargo check`

## Usage

### Running Hooks Manually

#### Using prek (Recommended)

Run all hooks on all files:
```bash
prek run --all-files
```

Run specific hooks:
```bash
prek run rustfmt
prek run rust-clippy
prek run cargo-deny
```

Run multiple specific hooks:
```bash
prek run rustfmt cargo-deny rust-clippy
```

Run hooks for files in a specific directory:
```bash
prek run --directory backend
```

Run hooks for files changed in the last commit:
```bash
prek run --last-commit
```

#### Using pre-commit (Alternative)

Run all hooks on all files:
```bash
pre-commit run --all-files
```

Run specific hook:
```bash
pre-commit run rustfmt
pre-commit run rust-clippy
pre-commit run cargo-deny
```

### Running Hooks on Changed Files Only

Hooks run automatically on `git commit` and `git push`, but you can also run them manually:

```bash
# Using prek
prek run

# Using pre-commit
pre-commit run
```

### Updating Hooks

Update all hooks to their latest versions:

```bash
# Using prek
prek autoupdate

# Using pre-commit
pre-commit autoupdate
```

## Configuration

The configuration is in `.pre-commit-config.yaml`. Key settings:

- **fail_fast: false**: Continue running all hooks even if one fails
- **minimum_pre_commit_version: "3.0.0"**: Requires pre-commit 3.0.0 or higher
- **files**: Only runs on `.rs`, `.toml`, `.lock`, `.yaml`, `.yml`, `.json` files
- **exclude**: Excludes `backend/target/`, `.git/`, `.cargo/`, `backend/Cargo.lock`, and other non-backend directories

## Troubleshooting

### Common Issues

1. **"cargo +nightly fmt" not found**: Ensure nightly toolchain is installed
2. **"cargo-deny" not found**: Install with `cargo install cargo-deny`
3. **"cargo-audit" not found**: Install with `cargo install cargo-audit`

### Skipping Hooks

To skip hooks for a specific commit:
```bash
git commit --no-verify -m "commit message"
```

### Updating Hook Versions

To update specific hook versions, edit `.pre-commit-config.yaml` and run:
```bash
pre-commit autoupdate
```

## CI Integration

To run these hooks in CI, add this step to CI configuration:

### Using prek (Recommended)

```yaml
- name: Run prek hooks
  run: |
    # Install prek
    curl --proto '=https' --tlsv1.2 -LsSf https://github.com/j178/prek/releases/download/v0.2.0-alpha.3/prek-installer.sh | sh
    # Run hooks
    prek run --all-files
```

### Using pre-commit (Alternative)

```yaml
- name: Run pre-commit hooks
  run: |
    pip install pre-commit
    pre-commit run --all-files
```

## Adding New Hooks

To add new hooks, edit `.pre-commit-config.yaml` and add them to the appropriate repository section. Then run:

```bash
# Using prek (recommended)
prek install
prek run --all-files

# Using pre-commit (alternative)
pre-commit install
pre-commit run --all-files
```


