---
title: "Helix — modal Rust editor with built-in LSP and tree-sitter"
main_link: https://helix-editor.com/
keywords: [helix, editor, modal, kakoune, rust, lsp, tree-sitter, multi-cursor]
status: reviewed
---

# Helix — modal Rust editor with built-in LSP and tree-sitter

**Main link:** <https://helix-editor.com/>

Repo: <https://github.com/helix-editor/helix>

## Summary

Helix is a modal terminal text editor written in Rust, inspired by Vim and (more directly) by [Kakoune](https://kakoune.org/). It ships with LSP, tree-sitter, multi-cursor selection, fuzzy file picker, global search, and a which-key-style hint popup — *out of the box*, with no plugin manager. The whole thing is configured in a few lines of TOML.

## Insight

The editing model is **selection-then-action** (Kakoune-style), not Vim's verb-then-noun. So you say `wd` ("select-word, delete") instead of `dw` ("delete-word"). It feels backwards for two days and then becomes natural — and you get a free multi-cursor model: every operation acts on every selection, so multi-edit is just `C` (add cursor below) and type.

Where Helix beats Neovim:

- Zero plugin churn. LSP, tree-sitter, formatting, completion, diagnostics, file picker — all built in.
- Discoverable. Pressing a leader-style key shows what comes next.
- Configuration fits on a postcard.

Where Helix loses:

- **No plugin system yet** (steel/scheme-based runtime is in progress on master but not stable). If you depend on copilot.lua, magit-style git, dap.nvim, debuggers, or org-mode, you can't get them — yet.
- LSP customisation is shallower than Neovim's; weird workspace setups can be painful.
- Macros and registers are different enough that muscle memory transfers poorly from Vim.

Picker:

- **Want a Vim-like editor that needs no setup**: Helix.
- **Want infinite extensibility and don't mind a config-as-a-side-project**: Neovim.
- **Want a GUI with the same modal feel and AI built in**: [Zed](https://zed.dev/) (also Rust, also has LSP/tree-sitter, GUI rather than TUI).
- **Want the original**: [Kakoune](https://kakoune.org/).

## Similar / related topics

- [Kakoune](https://kakoune.org/) — the editor whose model Helix borrows.
- [Neovim](https://neovim.io/) — the configurable Vim with a vast plugin ecosystem.
- [Zed](https://zed.dev/) — Rust GUI editor in a similar spirit.
- [Tree-sitter](https://tree-sitter.github.io/) — incremental parsing library Helix uses for syntax & selection.
- [[fish]] — same "great defaults, almost no config" philosophy.

## Internal links

- [[fish]]
- [[lazygit]]
- [[tools/linux/tmux|tmux]]
- [[zellij]]

## Keywords

`#helix` `#editor` `#modal` `#kakoune` `#rust` `#lsp` `#tree-sitter` `#multi-cursor` `#tui`

## References / raw notes

### Why I switched from Neovim

My Neovim config had 21 external plugins. Making LSP, tree-sitter, and formatting work took a while (LSP alone needed 3 plugins) and in the end there were still things that didn't work.

I switched to Helix, which can do so much out of the box. A non-exhaustive list:

- **LSP** including autocompletion, signature help, go-to-definition, find references — just works.
- **Tree-sitter** built in; you can even make selections on tree-sitter objects.
- **File picker** and **global search**.
- Pressing a key in normal mode shows subsequent keys you can press, and what they do.
- Jump to any visible word, add/remove/replace quotes or other surrounding characters.
- … and much more.

### My entire config

```toml
theme = "kanagawa"

[editor]
line-number = "relative"
cursorline = true
rulers = [80]
```

5 lines.

### One thing to get used to

Helix follows a **selection → action** model, so to delete the next word you press `wd` rather than `dw`. After a couple of days this is natural and unlocks the multi-cursor model.

### Install

```shell
# macOS
brew install helix

# Arch
sudo pacman -S helix

# From source
git clone https://github.com/helix-editor/helix
cd helix && cargo install --path helix-term --locked
```
