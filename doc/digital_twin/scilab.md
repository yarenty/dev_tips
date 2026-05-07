---
title: Scilab — open-source numerical computation
main_link: https://www.scilab.org/
keywords: [scilab, xcos, numerical-computation, matlab-alternative, simulation, open-source]
status: reviewed
review_date: 2026/05/03
---

# Scilab — open-source numerical computation

**Main link:** <https://www.scilab.org/>

## Summary

**Scilab** is a free, open-source MATLAB-style numerical-computation environment (originally INRIA, now Dassault Systèmes / ESI Group). It ships its own scripting language, a large standard library for linear algebra, signal processing, optimisation, and statistics, and **Xcos** — a graphical block-diagram editor for modelling and simulating dynamical systems (Scilab's answer to Simulink). Cross-platform, GPL/CeCILL-licensed.

## Insight

Scilab matters in this folder because Xcos is a **realistic open-source path to building a digital-twin model** of mechanical, thermal, electrical, or hydraulic systems without paying for MATLAB/Simulink. The language and toolboxes overlap with MATLAB enough that most academic textbooks "just work" with minor syntax tweaks; the community is smaller, the polish is lower, but the price is zero. Reach for it when (a) you need symbolic/numerical sim of a dynamical system, (b) the target is research / teaching / one-off engineering rather than a long-term platform commitment, and (c) MATLAB licensing is a non-starter. Pair with [[simulink|Simulink]] notes for context if you're moving from one to the other.

For real digital-twin operations at industrial scale, Scilab is undersized — see [[nvidia_omniverse]] or [[bmw|BMW iFACTORY]] for the upper end.

## Similar / related topics

- [[simulink]] — Commercial counterpart from MathWorks; the industry standard for control-system block-diagram simulation.
- **GNU Octave** — closer to MATLAB's *language* (most `.m` files run unmodified); no built-in block-diagram editor.
- **OpenModelica** — open-source Modelica implementation; better choice for *equation-based* multi-domain physical modelling.
- **Julia** + ModelingToolkit.jl — modern alternative for numerically-heavy simulation work.
- **Python** + SciPy + control-systems-library — most-used open alternative for new projects.

## Internal links

- [[simulink]] — Commercial counterpart in this folder.
- [[definition]] — Conceptual background on digital twins.
- [[nvidia_omniverse]] — Industrial-scale alternative tooling stack.

## Keywords

`#scilab` `#xcos` `#numerical-computation` `#matlab-alternative` `#simulation` `#open-source`

## References / raw notes

- Project site: <https://www.scilab.org/>
- Download: <https://www.scilab.org/download>
- Xcos overview: <https://www.scilab.org/about/scilab/xcos>
- Tutorials & docs: <https://www.scilab.org/learn>
- Scilab on GitLab: <https://gitlab.com/scilab/scilab>

### Quick orientation

| Component | What |
|-----------|------|
| Scilab CLI / IDE | Console + script editor + variable browser |
| Scilab language | MATLAB-like syntax (not 100 % compatible) |
| Standard library | Linear algebra, FFT, optimisation, ODE/PDE solvers, statistics |
| **Xcos** | Block-diagram modelling/simulation (analogous to Simulink) |
| ATOMS package manager | Community packages — control, image processing, etc. |
| Toolboxes | Numeric optimisation, signal proc, statistics, embedded codegen |

### Typical usage in a digital-twin context

1. Build the **plant model** in Xcos as a block diagram (mechanical / electrical / thermal blocks).
2. Add a **controller** block-diagram around it (PID, state-space, MPC).
3. Run simulations with parameter sweeps to characterise behaviour.
4. Export the model as C code (Xcos code-gen) for the embedded target.
5. (For digital-twin sync) feed real sensor data via Scilab's serial / TCP / OPC bindings into the Xcos simulation to compare predicted vs measured.
