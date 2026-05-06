---
title: "Void — open-source Cursor / Windsurf alternative"
main_link: https://github.com/voideditor/void
keywords: [void, editor, ai-editor, cursor, windsurf, vscode, local-llm, open-source]
status: reviewed
---

# Void — open-source Cursor / Windsurf alternative

**Main link:** <https://github.com/voideditor/void>

Site: <https://voideditor.com/>

## Summary

Void is an open-source AI editor built as a fork of VS Code, positioned explicitly as a Cursor/Windsurf alternative. It supports any model — hosted (OpenAI, Anthropic, Gemini) or local (Ollama, LM Studio, OpenAI-compatible endpoints) — and keeps your prompts/data on your machine by default. Familiar VS Code keymaps and most extensions Just Work.

## Insight

The pitch is simple: you already know VS Code, you already pay for an LLM (or you run one locally), so why pay $20/month for a closed editor on top? Void closes that gap.

When to reach for it:

- You want **Cursor-style** Composer/Edit/Chat flows but on your own infra.
- You need to point an editor at a **local model** (Ollama / LM Studio / llama.cpp) for offline / compliance reasons.
- You'd rather audit and fork the editor than trust a closed company's data-handling promises.

Caveats:

- Newer project — sharper edges than Cursor in places, especially around indexing huge monorepos.
- Extension compatibility is broad but not 100%; some Microsoft-licensed extensions (Pylance, remote-SSH) are restricted by upstream license.
- The "BYOK" experience is the headline feature; the polish that makes Cursor feel magical (tab-completion model tuning, agent loops) is catching up.

Practical comparison points: **Cursor** for the most polished UX, **Zed** if you want a Rust-native fast editor with growing AI features, **Continue.dev** if you'd rather keep stock VS Code and bolt AI on as an extension.

## Similar / related topics

- [Cursor](https://www.cursor.com/) — the closed-source benchmark.
- [Windsurf](https://codeium.com/windsurf) — Codeium's competitor.
- [Zed](https://zed.dev/) — Rust-native editor with AI baked in.
- [Continue.dev](https://www.continue.dev/) — open-source AI extension for VS Code/JetBrains.
- [aider](https://aider.chat/) — terminal-based AI pair-programming.

## Internal links

<!-- reviewed -->

- [[intellij]] — heavyweight IDE alternative when refactoring matters more than AI.
- [[markitdown]] — feed proprietary docs to your in-editor LLM.
- [[helix]] — terminal modal editor for the opposite end of the UX spectrum.
- [[mdfried]] — preview project Markdown alongside.

## Keywords

`#void` `#editor` `#ai-editor` `#cursor` `#windsurf` `#vscode` `#local-llm` `#open-source` `#editors`

## References / raw notes

> Local / cursor / windsurf

Repo: <https://github.com/voideditor/void>
