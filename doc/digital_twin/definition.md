---
title: Digital twin — definition & taxonomy
main_link: https://www.ibm.com/topics/what-is-a-digital-twin
keywords: [digital-twin, iot, simulation, taxonomy, industry-4.0, mbse]
status: reviewed
---

# Digital twin — definition & taxonomy

**Main link:** <https://www.ibm.com/topics/what-is-a-digital-twin>

## Summary

A **digital twin** is a virtual representation of a physical object, system, or process — kept continuously synchronised with its real-world counterpart via sensor data — used for simulation, what-if analysis, monitoring, and decision support across the asset's full lifecycle. The concept was formalised by Michael Grieves (2002, U. Michigan) and named by NASA's John Vickers (2010), but NASA was already doing it conceptually in the 1960s with earthbound replicas of Apollo spacecraft.

A digital twin differs from a *simulation* in two important ways: (1) it's bidirectional — sensor data flows in, insights flow back; (2) it's a *living model* spanning the asset's lifecycle, not a one-shot study.

## Insight

Reach for the digital-twin pattern when the **cost of building & operating a virtual model is lower than the cost of being wrong about the physical asset**. That equation works for jet engines, factories, large buildings, power generation, and aircraft — and basically nothing else. For a desk lamp it's overkill; for a Channel Tunnel it's table stakes.

The taxonomy is genuinely useful: **component → asset → system → process** twins are nested levels of abstraction (motor → engine → aircraft → fleet operations). Most industrial deployments end up needing all four levels eventually.

The hard part isn't the visualisation; it's the **continuous sync** — sensor topology, data ingestion at scale, model drift handling, and (lately) physics-informed ML to fuse simulation output with measured data.

## Similar / related topics

- [[cps]] — Cyber-Physical Systems: the academic / Industry 4.0 framing of the same underlying idea.
- [[isc2022]] — NVIDIA + BMW keynote on supercomputing-scale digital twins.
- [[nvidia_omniverse]] — NVIDIA's commercial platform for building digital twins.
- [[bmw]] — Concrete deployment example: BMW iFACTORY.
- MBSE (Model-Based Systems Engineering) — the systems-engineering discipline digital twins plug into. See IBM Engineering Systems Design / Rhapsody.

## Internal links

- [[cps]] — Academic CPS framing of the same concept.
- [[isc2022]] — Supercomputing-scale digital-twin keynote (NVIDIA + BMW, ISC 2022).
- [[bmw]] — Real-world deployment study.
- [[nvidia_omniverse]] — Tooling platform.
- [[links]] — Curated reading list for further depth.

## Keywords

`#digital-twin` `#iot` `#simulation` `#taxonomy` `#industry-4.0` `#mbse`

## References / raw notes

- IBM "What is a Digital Twin?": <https://www.ibm.com/topics/what-is-a-digital-twin>
- IBM IoT cheat-sheet (the original blog): <https://www.ibm.com/blogs/internet-of-things/iot-cheat-sheet-digital-twin/>
- IBM Digital Twin Exchange: <https://www.ibm.com/products/digital-twin-exchange>

### Memorisable definition

> A digital twin is a virtual representation of an object or system that spans its lifecycle, is updated from real-time data, and uses simulation, machine learning and reasoning to help decision-making.  — IBM

### Twin vs. simulation

| | Simulation | Digital twin |
|---|---|---|
| Data flow | One-way (model → result) | Two-way (sensors → model → insights → physical) |
| Time scope | One-shot study | Continuous over the asset lifecycle |
| Granularity | Single process | Multiple processes / systems |
| Real-time data | Optional | Core requirement |
| Sensor topology | N/A | First-class: every IoT sensor has a binding |

### Taxonomy (4 levels)

1. **Component / part twin** — single component (motor, valve, sensor).
2. **Asset twin** — interactions between components forming a usable asset (a pump assembly).
3. **System / unit twin** — assets working together (a chiller plant).
4. **Process twin** — multiple systems (an entire factory or supply chain).

Most real deployments use multiple levels in the same model.

### History

- **1991** — David Gelernter's *Mirror Worlds* introduces the conceptual framing.
- **2002** — Michael Grieves applies it to manufacturing (U. Michigan).
- **2010** — NASA's John Vickers coins the term *digital twin*.
- (1960s — NASA implicitly used the pattern with earthbound Apollo replicas, predating the formal name by decades.)

### Markets that benefit (IBM analysis)

- Engineering (systems)
- Automobile manufacturing
- Aircraft production
- Railcar design
- Building construction
- Manufacturing
- Power utilities
- Healthcare (patient telemetry)
- Urban planning (3D + AR overlays)

### "Cognitive digital twin"

Adds NLP, vision, ML, acoustic analytics, and signal processing on top of the core twin so the model can refine its own simulation choices (which tests to keep running, which to retire) — this is where physics-informed ML (e.g. NVIDIA Modulus, see [[nvidia_omniverse]]) and large-model integration are heading.

### Source article (verbatim, condensed)

The full original IBM blog and topic page are linked above; this article distils them. Headlines preserved:

- *"Digital twins let us understand the present and predict the future"*
- *"Use cases for digital twin: an engineer's point of view"*
- *"Unprecedented control over visualization, from afar"*
- *"The future of the cognitive digital twin"*

Market size data point (IBM, 2020): digital-twin market valued at USD 3.1 B; some analysts projected USD 48.2 B by 2026.
