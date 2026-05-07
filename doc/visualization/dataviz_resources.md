---
title: Data-viz resources — books, courses, references
main_link: https://datavizproject.com/
keywords: [dataviz, books, courses, tufte, knaflic, yau, chart-design, references]
status: reviewed
review_date: 2026/05/03
---

# Data-viz resources — books, courses, references

**Main link:** <https://datavizproject.com/>

## Summary

A short curated reading / reference list for *the craft* of data visualisation — picking the right chart type, communicating clearly, avoiding chartjunk — independent of any specific tool. Use this list to develop taste; use [[plotters]] / [[svelvet]] / [[ggtech]] / [[manim]] / [[visualization/grafana|grafana]] / [[diagrams]] / [[rerun]] etc. to actually draw the thing.

## Insight

The classic mistake is to learn one tool deeply (matplotlib, ggplot2, Tableau, ...) and assume the chart-design problem is solved. It isn't — every tool defaults to ugly because the *defaults* are written by the library author, not by a designer. The books below teach the *design* layer, which transfers between every tool you'll ever touch.

If you only have time for one thing: read **Storytelling with Data** (Knaflic) — it's the most directly actionable for "I have to send a deck on Monday and the charts in it have to convince someone".

## Similar / related topics

- [[portfolios]] — practitioners' Tableau Public portfolios for studying finished work.
- [[pudding]] — *The Pudding* — long-form data journalism; gold standard for interactive essays.
- [[ggtech]] — branded chart aesthetics from tech companies.
- [Datawrapper Academy](https://academy.datawrapper.de/) — short, focused articles on chart design (free).
- [Storytelling with Data podcast](https://www.storytellingwithdata.com/podcast) — companion to Knaflic's books.

## Internal links

- [[portfolios]] — finished portfolios to study
- [[pudding]] — best-in-class interactive data journalism
- [[ggtech]] — examples of branded "good defaults"

## Keywords

`#visualization` `#dataviz` `#books` `#courses` `#chart-design` `#references` `#tufte` `#knaflic`

## References / raw notes

### Books I'd actually buy

| Book | Author | Why |
|------|--------|-----|
| **Storytelling with Data** | Cole Nussbaumer Knaflic | <https://www.amazon.co.uk/dp/1119002257> — the most directly actionable book on this list; pre-attentive attributes, slide-deck-ready charts, "what to focus the eye on". |
| **Visualize This** | Nathan Yau | <https://www.amazon.co.uk/dp/0470944889> — the practical "how to actually make these in code" companion (R + JavaScript); pairs well with Knaflic's design focus. |
| **The Visual Display of Quantitative Information** | Edward Tufte | <https://www.edwardtufte.com/tufte/books_vdqi> — the canonical text. Read for the *vocabulary* (chartjunk, data-ink ratio, small multiples) even if the examples feel dated. |
| **Information Dashboard Design** | Stephen Few | the right book for anyone building a Grafana / Looker / Tableau dashboard for an operations team. |
| **Functional Art** | Alberto Cairo | the journalist's perspective; lots on what *not* to do. |

### Courses / interactive references

- **DataViz Project** — searchable catalogue of chart types with descriptions and use-cases: <https://datavizproject.com/>
- **From Data to Viz** — flowchart that picks a chart type from your data shape: <https://www.data-to-viz.com/>
- **Datawrapper Academy** — short focused articles, free: <https://academy.datawrapper.de/>
- **Microsoft Learn — How to create effective charts and diagrams**: <https://education.microsoft.com/nb-no/course/0a60eeb6/0>
- **Storytelling with Data** workshops + podcast: <https://www.storytellingwithdata.com/>

### Quick gut-check before publishing a chart

1. Can you read it from across the room? (size, contrast, font)
2. Is the headline a *finding*, not a label? ("Sales up 40% in Q3" beats "Quarterly Sales".)
3. Have you removed everything that doesn't carry information? (gridlines, 3D effects, drop shadows, redundant legends)
4. Is your colour choice colour-blind safe? Test with [Sim Daltonism](https://michelf.ca/projects/sim-daltonism/) or [Coblis](https://www.color-blindness.com/coblis-color-blindness-simulator/).
5. Does the y-axis start at zero (when it should — bar charts always; line charts only when the range warrants)?
