---
title: Markitdown
main_link: https://github.com/microsoft/markitdown?utm_source=tldrai
keywords: [markitdown, editors, tools, text, openai, exif]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Markitdown

**Main link:** <https://github.com/microsoft/markitdown?utm_source=tldrai>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[mdfried]] — Mdfried _(score 26.4)_
- [[void]] — VOID _(score 26.4)_
- [[intellij]] — IntelliJ productivity tips _(score 26.4)_
- [[indexing]] — Indexing _(score 15.0)_
- [[prek]] — Prek _(score 15.0)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#markitdown` `#editors` `#tools` `#text` `#openai` `#exif` `#install`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Markitdown


https://github.com/microsoft/markitdown?utm_source=tldrai




MarkItDown is a utility for converting various files to Markdown (e.g., for indexing, text analysis, etc). It supports:

    PDF
    PowerPoint
    Word
    Excel
    Images (EXIF metadata and OCR)
    Audio (EXIF metadata and speech transcription)
    HTML
    Text-based formats (CSV, JSON, XML)
    ZIP files (iterates over contents)
To install MarkItDown, use pip: pip install markitdown. Alternatively, you can install it from the source: pip install -e .

Usage
Command-Line
    markitdown path-to-file.pdf > document.md
Or use -o to specify the output file:

    markitdown path-to-file.pdf -o document.md
You can also pipe content:

cat path-to-file.pdf | markitdown
Python API
Basic usage in Python:
    
    from markitdown import MarkItDown
    
    md = MarkItDown()
    result = md.convert("test.xlsx")
    print(result.text_content)
To use Large Language Models for image descriptions, provide llm_client and llm_model:

    from markitdown import MarkItDown
    from openai import OpenAI
    
    client = OpenAI()
    md = MarkItDown(llm_client=client, llm_model="gpt-4o")
    result = md.convert("example.jpg")
    print(result.text_content)
