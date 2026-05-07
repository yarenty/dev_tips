---
title: ink (JavaScript / React for CLIs)
main_link: https://github.com/vadimdemedes/ink
keywords: [ink, javascript, react, cli, tui, vadimdemedes, jsx]
status: reviewed
review_date: 2026/05/03
---

# ink (JavaScript / React for CLIs)

**Main link:** <https://github.com/vadimdemedes/ink>

## Summary

`ink` (by Vadim Demedes) is a **JavaScript** (Node.js) library that lets you build interactive CLIs and small TUIs using React components and JSX. You write `<Box>`, `<Text>`, `<Newline>`, `<Spinner>`, etc., and ink renders them to the terminal with proper layout (Yoga/Flexbox), focus, input handling, and re-rendering on state change. It's the design ancestor of Rust's [[iocraft]] — the API shape, naming, and overall philosophy come straight from here.

## Insight

This article is filed under `programming/rust/tui/` despite **not being a Rust library** because the original raw notes lived in the Rust TUI section and explicitly described it as *"more cool CLI than TUI"* — and because it's the obvious comparison point any time `iocraft` comes up. If you're a Rust developer, the practical takeaways are:

- **Don't reach for ink from Rust.** It's Node-only. Use [[iocraft]] for the same shape in Rust.
- **Do read the [ink README](https://github.com/vadimdemedes/ink) and the [showcase apps](https://github.com/vadimdemedes/ink#whos-using-ink)** if you want to understand what the declarative-CLI niche actually feels like — Cloudflare Wrangler, GitHub Copilot CLI, Gatsby's CLI, Prisma's, Tailwind v4's installer, etc. all use it.
- **It's the right tool** if you're already in the Node ecosystem and need a beautiful CLI. The story for "rich CLI in JS" is actually better than in Rust right now.

The killer-app pattern: ephemeral, non-fullscreen output (a checklist that ticks off as steps complete, a progress dashboard that exits cleanly), not a long-lived fullscreen TUI like an editor.

## Similar / related topics

- [[iocraft]] — the Rust port of the same design philosophy.
- [[../tui/README|Ratatui]] — Rust's mainstream imperative TUI.
- [`@inquirer/prompts`](https://github.com/SBoudrias/Inquirer.js) — the Node prompt library; pairs with ink the way [[../cli/dialoguer|dialoguer]] pairs with [[../tui/README|Ratatui]] in Rust.
- [`oclif`](https://oclif.io/) — Salesforce's Node CLI framework; often used alongside ink for the rendering layer.

## Internal links
- [[iocraft]] — the Rust take.
- [[../tui/README|Rust TUI]] — section landing.
- [[../cli/README|Rust CLI]] — for the equivalent prompt/progress story in Rust.

## Keywords

`#javascript` `#react` `#cli` `#tui` `#ink` `#jsx` `#node`

## References / raw notes

- Repo: <https://github.com/vadimdemedes/ink>
- Showcase: <https://github.com/vadimdemedes/ink#whos-using-ink>

> Original note: *"this is more cool CLI than TUI"* — preserved verbatim because it's the most accurate one-line description of where ink shines (rich, ephemeral, non-fullscreen CLI output) vs. what people usually mean by TUI (long-lived fullscreen apps).
