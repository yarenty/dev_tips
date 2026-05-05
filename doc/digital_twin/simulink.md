---
title: Simulink — MathWorks block-diagram simulation
main_link: https://www.mathworks.com/products/simulink.html
keywords: [simulink, matlab, mathworks, block-diagram, control-systems, model-based-design, simscape]
status: reviewed
---

# Simulink — MathWorks block-diagram simulation

**Main link:** <https://www.mathworks.com/products/simulink.html>

## Summary

**Simulink** is MathWorks' graphical block-diagram environment for modelling, simulating, and analysing dynamic systems, layered on top of MATLAB. The de-facto industry standard in **control systems, automotive, aerospace, and embedded** for building plant models, designing controllers, and generating production-quality C/C++/HDL code from those models. Comes with a deep ecosystem of toolboxes — **Simscape** (multi-physical modelling), **Stateflow** (state machines), **Simulink Real-Time** (HIL), **Embedded Coder** (codegen) — and integrates with MathWorks' digital-twin / predictive-maintenance training (see [[links]]).

## Insight

If your domain has any history in control systems or automotive, Simulink is the **incumbent everyone benchmarks against**. It's how you do *Model-Based Design*: build a plant model and a controller in the same canvas, simulate, then generate certified embedded code with a single button. The strengths are real: massive validated library coverage, world-class numerical solvers, ISO 26262 / DO-178C qualifiable code generators, and a hire-anywhere skill set.

The trade-offs are equally real: it's expensive (per-seat per-toolbox, and the toolboxes you need always seem to be different ones), the file format is binary so version control is awkward, and "real" digital-twin operations (real-time sensor sync to a running model on a server) require Simulink Real-Time + a target box. For a free path, see [[scilab]]; for industrial-scale visualisation/operations, see [[nvidia_omniverse]].

In this folder, Simulink fits the **engineering / plant-model** niche of the digital-twin spectrum (build the model, simulate, generate code), while Omniverse owns the operational-twin / 3D-collaborative niche.

## Similar / related topics

- [[scilab]] — Open-source counterpart with Xcos as its block-diagram editor.
- **Modelica / Dymola / OpenModelica** — equation-based multi-domain alternative; preferred when first-principles multi-physics modelling is the priority.
- **LabVIEW** — competing graphical-language environment; stronger in test-and-measurement, weaker in modelling.
- **Julia + ModelingToolkit.jl** — modern open challenger.
- **Ansys Twin Builder** — ANSYS's commercial digital-twin platform (different vendor, similar use case).

## Internal links

<!-- reviewed -->

- [[scilab]] — Open-source counterpart in this folder.
- [[definition]] — Conceptual digital-twin background.
- [[links]] — MathWorks's digital-twin training course is in the curated list.
- [[nvidia_omniverse]] — Operational-scale tooling stack alternative.

## Keywords

`#simulink` `#matlab` `#mathworks` `#block-diagram` `#control-systems` `#model-based-design` `#simscape`

## References / raw notes

- Product page: <https://www.mathworks.com/products/simulink.html>
- Simscape (multi-physical modelling): <https://www.mathworks.com/products/simscape.html>
- Stateflow (state machines): <https://www.mathworks.com/products/stateflow.html>
- Simulink Real-Time: <https://www.mathworks.com/products/simulink-real-time.html>
- Embedded Coder (codegen): <https://www.mathworks.com/products/embedded-coder.html>
- Digital twins for predictive maintenance (training): <https://explore.mathworks.com/digital-twins-for-predictive-maintenance>

### Where Simulink fits in the digital-twin lifecycle

| Lifecycle stage | Simulink's role |
|---|---|
| Concept / requirements | System Composer + Requirements Toolbox |
| Plant modelling | Simulink + **Simscape** (mechanical, electrical, hydraulic, thermal) |
| Controller design | Simulink + Control System Toolbox + Model Predictive Control Toolbox |
| Verification | Simulink Test + Simulink Coverage |
| Code generation | **Embedded Coder** → C/C++ for ECU; HDL Coder → FPGA |
| Real-time HIL | Simulink Real-Time + Speedgoat target |
| Operational twin | Simulink Compiler + ThingSpeak / cloud deployment + sensor data feed |

### Practical gotchas

- **Version control**: `.slx` is a binary zip; use `slxupdate`, `Simulink Project`, and `slcompare` for diffs/merges. Never store secrets inside model parameters; use `Data Dictionaries` instead.
- **Reproducibility**: solver choice and step size matter; pin them in the model and CI.
- **Licensing**: per-toolbox, per-seat. Audit which toolboxes your model actually depends on with `Simulink Manifest Tools` before committing.
