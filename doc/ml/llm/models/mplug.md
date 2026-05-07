---
title: mPLUG — Alibaba's vision-language model family (mPLUG / mPLUG-2 / mPLUG-Owl)
main_link: https://github.com/X-PLUG
keywords: [mplug, mplug-owl, mplug-docowl, alibaba, alicemind, damo, multimodal, vision-language]
status: reviewed
review_date: 2026/05/03
---

# mPLUG — Alibaba's vision-language model family (mPLUG / mPLUG-2 / mPLUG-Owl)

**Main link:** <https://github.com/X-PLUG>

## Summary

mPLUG is a family of vision-language and multimodal-LLM models from Alibaba's DAMO Academy, originally released as part of the **AliceMind** open-source NLP suite. The lineage runs **mPLUG** (2022, encoder-decoder vision-language model with cross-modal skip-connections) → **mPLUG-2** (Mar 2023, modular shared-and-specialised architecture across text/vision/video) → **mPLUG-Owl / Owl2 / Owl3** (2023-2024, multimodal LLMs with progressively better vision-text alignment) → **mPLUG-DocOwl** (OCR-free document understanding family). All the modern work lives under the [`X-PLUG` GitHub org](https://github.com/X-PLUG); the historical AliceMind monorepo is in [`alibaba/AliceMind`](https://github.com/alibaba/AliceMind).

## Insight

mPLUG matters mostly as the **document-understanding** specialist in the open-weights vision-language space — DocOwl is one of the better OCR-free choices when you need to extract structure from scanned docs, screenshots, or rich PDFs without running a separate OCR pass. For general-purpose VLM work, Alibaba's own **Qwen-VL / Qwen2.5-VL** has largely eclipsed mPLUG-Owl (same company, more attention), and the broader landscape (LLaVA, InternVL, MiniCPM-V, Pixtral) is highly competitive.

Naming gotcha: "mPLUG" alone is ambiguous — it can mean the original 2022 paper, the 2023 mPLUG-2, or the whole umbrella. Treat the **X-PLUG GitHub org** as the canonical entry point and the AliceMind suite as the historical context. Don't reach for mPLUG just because it shows up on a leaderboard — for production multimodal work in 2025 the practical picks are Qwen2.5-VL (open weights) or Claude / Gemini / GPT-4o (hosted).

## Similar / related topics

- Qwen-VL / Qwen2.5-VL — Alibaba's own Qwen-side VLM line; the modern flagship from the same company.
- LLaVA — the canonical open-weights VLM lineage (LLaVA-1.5 / 1.6 / NeXT / OneVision); Microsoft + UW-Madison.
- InternVL — Shanghai AI Lab's strong open VLM family, often beats mPLUG-Owl at the same size.
- MiniCPM-V — small (8B) VLM from OpenBMB punching above its weight.
- Donut / Nougat / Marker — competing OCR-free document-understanding pipelines (smaller, single-purpose).

## Internal links
- [[README|llm/models]]
- [[../README|llm]]
- [[llama]]
- [[deepseek]]

## Keywords

`#mplug` `#mplug-owl` `#mplug-docowl` `#alibaba` `#alicemind` `#damo` `#multimodal` `#vision-language`

## References / raw notes

### Canonical entry points

- X-PLUG GitHub org (modern home): <https://github.com/X-PLUG>
- AliceMind monorepo (historical, broader NLP suite): <https://github.com/alibaba/AliceMind>

### mPLUG-DocOwl — OCR-free document understanding

<https://github.com/X-PLUG/mPLUG-DocOwl>

The Powerful Multi-modal LLM Family for OCR-free Document Understanding. Aimed at understanding documents (invoices, scientific papers, screenshots, slides) directly from pixels without an upstream OCR step. Useful comparison points: Donut, Nougat, Marker, and the document-mode of Qwen2.5-VL.

### mPLUG-Owl — multimodal LLM line

<https://github.com/X-PLUG/mPLUG-Owl>

The mPLUG-Owl chat-style multimodal-LLM repo, including:

- mPLUG-Owl (2023) — original.
- mPLUG-Owl2 — improved vision-language alignment.
- mPLUG-Owl3 (<https://github.com/X-PLUG/mPLUG-Owl/tree/main/mPLUG-Owl3>) — long-image-sequence and video-friendly variant.

### AliceMind / mPLUG 2.0

<https://github.com/alibaba/AliceMind>

DAMO Academy's open-source NLP suite ("**Ali**baba's Collection of **E**ncoder-decoders from **MinD**"). Hosts the original mPLUG and mPLUG-2 code. Now mostly historical — the active development moved to the X-PLUG org and to the Qwen line for general-purpose multimodal work.
