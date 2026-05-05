---
title: Vitis
main_link: https://support.xilinx.com/s/article/76616?language=en_US
keywords: [vitis, xilinx, dpu, linux, mine]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Vitis

**Main link:** <https://support.xilinx.com/s/article/76616?language=en_US>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[limbo]] — Limbo _(score 16.3)_
- [[mosh]] — Mosh _(score 16.3)_
- [[tools/linux/tmux|tmux]] — TMUX _(score 16.3)_
- [[terminals]] — Project Summary: Byobu, Mosh, Xpra _(score 16.3)_
- [[programming/rust/tooling/tools|tools]] — coreutils _(score 16.3)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#vitis` `#xilinx` `#dpu` `#linux` `#install` `#installation` `#mine`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

Just installed Vitis on linux mint.

If you plan to install Vitis on any ubuntu based linux, make sure to install libtinfo5 before starting the installer. https://support.xilinx.com/s/article/76616?language=en_US

I recommend to mount your /tools directory (or any other installation target) on a dedicated partition. This way, you can disable file system journaling in the hope of speeding up the installation process. Mine took 8 hrs on a SSD drive using ReiserFS with yournaling enabled.

It is also recommended to uncheck all components and parts you don't use, especially if you use an accelerator card, i.e. VCK5000, C1100 (--> no peta linux). The KV260 runs an embedded processor booting linux --> peta linux needed.

Only install Vitis (the uppermost radio button). Vivado comes with this.
