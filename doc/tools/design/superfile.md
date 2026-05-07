---
title: "superfile (spf) — pretty TUI multi-panel file manager"
main_link: https://github.com/MHNightCat/superfile
keywords: [superfile, spf, tui, file-manager, go, ranger-alternative, yazi, lf]
status: reviewed
review_date: 2026/05/03
---

# superfile (spf) — pretty TUI multi-panel file manager

**Main link:** <https://github.com/MHNightCat/superfile>

Tutorial wiki: <https://github.com/MHNightCat/superfile/wiki/Tutorial>

## Summary

`superfile` (binary name: `spf`) is a modern terminal file manager with a polished, multi-panel UI built on Bubble Tea. It supports pinning sidebar folders, multiple file panels, a process bar for ongoing copies/moves, a metadata panel, normal/select modes, and Nerd-Font icons throughout. Install with `brew install superfile`.

## Insight

The TUI-file-manager space is crowded — ranger, lf, nnn, broot, yazi — and the differentiation matters:

- vs **`ranger`** — Python, three-pane miller columns, mature; `spf` is faster and prettier but less extensible.
- vs **`lf`** — Go like `spf`, vim-keybinding minimalist; `spf` ships more features by default (pinning, process bar, metadata pane) at the cost of more chrome.
- vs **`nnn`** — C, ultra-minimal, plugin-driven; `spf` is the opposite vibe.
- vs **`yazi`** — Rust, async, image previews via Kitty/Sixel; `yazi` wins on previews and speed, `spf` wins on out-of-the-box polish.

Reach for `spf` when you want a TUI file manager that **looks good immediately** without spending a weekend in config files, and you have a Nerd Font installed. Skip it if you live by the principle of "boring tool, hand-crafted dotfiles" — `lf` or `ranger` will serve you better.

Watch out for: requires a Nerd Font; a few keybinding quirks (`H` is shift+h, etc.); selection mode (`v`) versus normal mode is something you _will_ mash by accident the first day.

## Similar / related topics

- [`ranger`](https://github.com/ranger/ranger) — the Python classic.
- [`lf`](https://github.com/gokcehan/lf) — minimalist Go file manager.
- [`yazi`](https://yazi-rs.github.io/) — fast Rust TUI with image previews.
- [`nnn`](https://github.com/jarun/nnn) — ultra-minimal C, plugin-driven.
- [[lstr]] — when you just want to look, not navigate.

## Internal links

- [[lstr]] — quick non-interactive tree view.
- [[czkawka]] — find duplicates / large files; `spf` to clean up.
- [[tools/linux/tmux|tmux]] — keep `spf` in a side pane.

## Keywords

`#superfile` `#spf` `#tui` `#file-manager` `#go` `#bubbletea` `#design` `#tools`

## References / raw notes

> SPF — `brew install superfile`
>
> Requires a Nerd Font.

Demo & tutorial: <https://github.com/MHNightCat/superfile/wiki/Tutorial>

### Default keybindings (cheat sheet)

**General**

| Function | Key | Variable |
|---|---|---|
| Start SuperFile | `spf` | |
| Reload | press any key to reload | |
| Quit | `esc`, `q` | `quit` |
| Open help menu | `?` | `open_help_menu` |

**Panel navigation**

| Function | Key | Variable |
|---|---|---|
| Pin / unpin folder to sidebar | `ctrl+p` | `pinned_folder` |
| Create new file panel | `ctrl+n` | `create_new_file_panel` |
| Close focused panel | `ctrl+w` | `close_file_panel` |
| Next panel | `tab`, `shift+right` | `next_file_panel` |
| Previous panel | `shift+left`, `H` | `previous_file_panel` |
| Focus process bar | `p` | `focus_on_process_bar` |
| Focus sidebar | `b` | `focus_on_side_bar` |
| Focus metadata | `m` | `focus_on_metadata` |

**Panel movement**

| Function | Key | Variable |
|---|---|---|
| Toggle select / normal mode | `v` | `change_panel_mode` |
| Up / down | `up`/`k`, `down`/`j` | `list_up`, `list_down` |
| Enter folder | `enter`, `l` | `select_item` |
| Parent folder | `h`, `backspace` | `parent_folder` |
| Select all (select mode) | `ctrl+a` | `file_panel_select_all_item` |
| Toggle dotfiles | `ctrl+h` | `toggle_dot_file` |
| Toggle search | `ctrl+f` | `search_bar` |

**File operations**

| Function | Key | Variable |
|---|---|---|
| New folder | `f` | `file_panel_folder_create` |
| New file | `c` | `file_panel_file_create` |
| Rename | `r` | `file_panel_item_rename` |
| Extract zip | `ctrl+e` | `extract_file` |
| Compress to zip | `ctrl+r` | `compress_file` |
| Delete | `ctrl+d` | `delete_item` |
| Copy | `ctrl+c` | `copy_single_item` |
| Cut | `ctrl+x` | `file_panel_select_mode_item_cut` |
| Paste | `ctrl+v` | `paste_item` |
| Open with default app | `enter`, `l` | `select_item` |
| Open with editor | `e` | `open_file_with_editor` |
| Open dir with editor | `E` | `current_directory_with_editor` |

**Pop-up modal**

| Function | Key | Variable |
|---|---|---|
| Confirm | `enter` | `confirm` |
| Cancel | `ctrl+c`, `esc` | `cancel` |
