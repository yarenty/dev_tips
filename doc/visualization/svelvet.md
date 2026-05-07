---
title: "Svelvet: node-graph editor for Svelte / web visualisations"
main_link: https://www.svelvet.io/
keywords: [svelvet, svelte, javascript, node-graph, visualisation, flow-editor]
status: reviewed
---

# Svelvet: node-graph editor for Svelte / web visualisations

**Main link:** <https://www.svelvet.io/>

Docs: <https://www.svelvet.io/docs/core-concepts/>

## Summary

[Svelvet](https://www.svelvet.io/) is a [Svelte](https://svelte.dev/) component library for building interactive node-graph editors in the browser — the kind of UI you see in Figma's flow tool, n8n, Blender's shader editor, or any "drag boxes around and connect them with lines" visual tool. You drop a `<Svelvet>` component into your Svelte app, hand it `nodes` and `edges` arrays, and it gives you the dragging, connecting, panning, and zooming for free.

## Insight

Almost every "visual programming" or "workflow editor" feature you'll be asked to build is shaped like a node-graph. Hand-rolling one of these is much more work than it looks (touch + mouse, panning vs. dragging, zoom-to-cursor, snap-to-grid, edge routing, port compatibility, undo/redo). Reach for an off-the-shelf component library instead.

The four serious choices in 2024:

| Library | Framework | Notes |
|---------|-----------|-------|
| [Svelvet](https://www.svelvet.io/) | Svelte | this article |
| [React Flow](https://reactflow.dev/) | React | the dominant pick; biggest community |
| [Vue Flow](https://vueflow.dev/) | Vue | port of React Flow |
| [LiteGraph.js](https://github.com/jagenjo/litegraph.js) | vanilla JS | older; what ComfyUI uses |

Pick by framework first; the feature sets are roughly comparable. Svelvet specifically has a smaller community than React Flow, so for production work React Flow is usually the safer bet unless you're already committed to Svelte.

## Similar / related topics

- [React Flow](https://reactflow.dev/) — the React-side equivalent; usually the default recommendation.
- [Vue Flow](https://vueflow.dev/) — same idea for Vue.
- [LiteGraph.js](https://github.com/jagenjo/litegraph.js) — vanilla JS, no framework dependency.
- [drawio / D2](https://www.drawio.com/) — for *static* diagrams rather than interactive editors. See [[diagrams]].
- [[plotters]] — the Rust-side answer for *non-interactive* chart rendering.

## Internal links

- [[diagrams]] — for static node-graph drawings (drawio / D2)
- [[plotters]] — Rust-side chart rendering

## Keywords

`#visualization` `#svelvet` `#svelte` `#javascript` `#node-graph` `#flow-editor`

## References / raw notes

- Site: <https://www.svelvet.io/>
- Core concepts docs: <https://www.svelvet.io/docs/core-concepts/>
- Repo: <https://github.com/open-source-labs/Svelvet>
