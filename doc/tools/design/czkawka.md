---
title: "Czkawka — find duplicate / similar / wasted files (Krokiet GUI)"
main_link: https://github.com/qarmin/czkawka
keywords: [czkawka, krokiet, duplicates, disk-cleanup, rust, similar-images, gtk]
status: reviewed
---

# Czkawka — find duplicate / similar / wasted files

**Main link:** <https://github.com/qarmin/czkawka>

New Slint-based GUI: <https://github.com/qarmin/czkawka/blob/master/krokiet/README.md>

## Summary

Czkawka (Polish for "hiccup", pronounced roughly _chkav-ka_) is a Rust-based, multi-platform disk-cleanup tool by Rafał Mikrut. It finds **duplicate files**, **similar images**, **similar videos**, **broken files**, **empty directories**, **empty files**, **temporary files**, **big files**, **invalid symlinks**, and **bad extensions**, all in one binary. CLI (`czkawka_cli`), GTK GUI (`czkawka_gui`), and the new Slint-based GUI **Krokiet** ("a small step" in Polish) are all maintained.

## Insight

This is the spring-cleaning tool. Twice a year I point it at `~/Pictures` and `~/Downloads`, watch it find ten thousand duplicates from old phone backups, and reclaim 30–80 GB without having to think.

Why it stands out:

- **All checks in one binary** — most rivals do duplicates _or_ similar images _or_ big-file scanning; Czkawka does all of it.
- **Perceptual image / video hashing** — finds visually similar files (resized, recompressed, slightly cropped) not just byte-identical ones.
- **Fast** — Rust, multi-threaded, sane I/O.
- **Free, open-source, no telemetry** — unlike CleanMyMac / DaisyDisk on the macOS side.
- The **Krokiet** GUI is the modern path forward; the older GTK UI still works but Slint renders better cross-platform.

Watch out for:

- Image-similarity is heuristic — _always_ review the matches before bulk-deleting.
- Default action is **delete**; prefer "move to trash" or "hardlink" mode the first time you use it.
- Large libraries can take a while to hash; let it run in the background.

## Similar / related topics

- [fclones](https://github.com/pkolaczk/fclones) — CLI-only Rust duplicate finder, very fast on huge trees.
- [rmlint](https://github.com/sahib/rmlint) — older C duplicate-finder with rich shell-script output.
- [dupeGuru](https://dupeguru.voltaicideas.net/) — cross-platform Python GUI; the historical reference.
- [DaisyDisk](https://daisydiskapp.com/) / [GrandPerspective](https://grandperspectiv.sourceforge.net/) — macOS visual-disk-usage view; complementary, not duplicate-focused.
- [[lstr]] — visualise the directory structure first.

## Internal links

- [[lstr]] — see the tree before you start deleting.
- [[superfile]] — TUI file-manager to follow up on findings.
- [[deskreen]] — mirror Krokiet to a second screen during long scans.

## Keywords

`#czkawka` `#krokiet` `#duplicates` `#disk-cleanup` `#rust` `#similar-images` `#gtk` `#design` `#tools`

## References / raw notes

> # Cleaning disk
>
> <https://github.com/qarmin/czkawka>
>
> Nice new GUI: <https://github.com/qarmin/czkawka/blob/master/krokiet/README.md>
