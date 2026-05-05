---
title: Vitis
main_link: https://support.xilinx.com/s/article/76616?language=en_US
keywords: [vitis, xilinx, dpu, linux, install, installation, mine]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Vitis

**Main link:** <https://support.xilinx.com/s/article/76616?language=en_US>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

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
