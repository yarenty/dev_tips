---
title: "MarkItDown — convert anything into Markdown"
main_link: https://github.com/microsoft/markitdown
keywords: [markitdown, microsoft, markdown, llm, pdf, docx, pptx, ocr, python]
status: reviewed
---

# MarkItDown — convert anything into Markdown

**Main link:** <https://github.com/microsoft/markitdown>

## Summary

`markitdown` is a Microsoft-maintained Python utility (CLI + library) that converts PDFs, Office documents (Word, Excel, PowerPoint), images (with EXIF + OCR), audio (EXIF + speech transcription), HTML, CSV/JSON/XML, and even ZIP archives into Markdown. The point isn't pretty rendering — it's producing clean, token-efficient text suitable for indexing, search, and feeding into LLMs.

## Insight

This is the "stop shipping PDFs to GPT" tool. Most LLM workflows degrade fast when raw PDFs/PPTX bytes go in; `markitdown` flattens them to Markdown that survives chunking and embedding far better. It's also a solid drop-in for static-site or knowledge-base ingestion when you have a heap of legacy `.docx` lying around.

Things to keep in mind:

- It's a **converter**, not a renderer or editor — output quality depends on the source. Image-heavy PDFs still need an OCR pass (it can use OpenAI for image descriptions, set `llm_client=`).
- Speech transcription needs an LLM/audio backend; not magic.
- Pairs naturally with [[mdfried]] for terminal preview and with any LLM that takes Markdown as input.

```bash
pip install markitdown                  # or: pip install -e .
markitdown deck.pptx > deck.md
markitdown report.pdf -o report.md
cat report.pdf | markitdown
```

```python
from markitdown import MarkItDown
md = MarkItDown()
print(md.convert("test.xlsx").text_content)

# With LLM-driven image descriptions
from openai import OpenAI
md = MarkItDown(llm_client=OpenAI(), llm_model="gpt-4o")
print(md.convert("example.jpg").text_content)
```

## Similar / related topics

- [Pandoc](https://pandoc.org/) — the universal document converter; broader format matrix, more knobs, less LLM-shaped output.
- [docling](https://github.com/DS4SD/docling) — IBM's heavier-weight PDF/document → Markdown/JSON pipeline with layout reconstruction.
- [unstructured](https://github.com/Unstructured-IO/unstructured) — RAG-flavoured ingestion library; richer chunking primitives.
- [[mdfried]] — preview converted Markdown in the terminal.
- [[typst]] — go the other way: structured text → PDF.

## Internal links

<!-- reviewed -->

- [[mdfried]] — preview the resulting Markdown.
- [[intellij]] — paste cleaned Markdown into your IDE-side LLM.
- [[void]] — same idea, AI-editor flavour.
- [[typst]] — typesetting from clean source instead.

## Keywords

`#markitdown` `#microsoft` `#markdown` `#llm` `#pdf` `#docx` `#pptx` `#ocr` `#python` `#editors`

## References / raw notes

Supported inputs: PDF, PowerPoint, Word, Excel, Images (EXIF + OCR), Audio (EXIF + speech), HTML, CSV, JSON, XML, ZIP (recursive).

Install:

```bash
pip install markitdown
# or from source
pip install -e .
```

CLI:

```bash
markitdown path-to-file.pdf > document.md
markitdown path-to-file.pdf -o document.md
cat path-to-file.pdf | markitdown
```

Python API:

```python
from markitdown import MarkItDown

md = MarkItDown()
result = md.convert("test.xlsx")
print(result.text_content)
```

LLM-assisted image descriptions:

```python
from markitdown import MarkItDown
from openai import OpenAI

client = OpenAI()
md = MarkItDown(llm_client=client, llm_model="gpt-4o")
result = md.convert("example.jpg")
print(result.text_content)
```
