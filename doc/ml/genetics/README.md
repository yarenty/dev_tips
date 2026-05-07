---
title: Genetics — evolutionary computation in ML
keywords: [genetics, evolutionary, ga, moea, neat, neuroevolution, automl]
status: reviewed
---

# Genetics — evolutionary computation in ML

A small section about **evolutionary / genetic algorithms** as used in ML and optimisation: multi-objective evolutionary algorithms (MOEAs) for engineering / OR problems, neuroevolution (NEAT, NES, CMA-ES) as a substitute for SGD when gradients aren't available, AutoML-via-evolution (TPOT, AutoML-Zero), and the classical population-based search techniques that underpin all of the above.

## Articles

- [The MOEA Framework — multi-objective evolutionary algorithms in Java](moea.md) — David Hadka's Java framework for multi-objective EAs (NSGA-II/III, MOEA/D, SPEA2, IBEA); plus the picker among Python alternatives (pymoo, DEAP, Platypus) and matlab PlatEMO.

## When evolutionary methods earn their keep

- **Non-differentiable objectives** — symbolic regression, program synthesis, hyperparameter search where the loss surface is rough, discrete, or simulation-based.
- **Multi-objective Pareto-front problems** — engineering / OR / supply-chain where you need a *set* of compromise solutions, not a single optimum.
- **Neuroevolution** — small RL / control problems where SGD doesn't apply (NEAT, HyperNEAT, NES, CMA-ES, OpenAI's evolution-strategies-as-a-scalable-alternative-to-RL line).
- **AutoML by evolution** — TPOT (scikit-learn pipelines), AutoML-Zero (rediscovers backprop from scratch).

## When *not* to reach for them

- Differentiable, well-behaved objectives — SGD wins on speed, sample efficiency, and theoretical guarantees.
- Most modern deep-learning problems — gradient methods + scaling have eaten this whole space.
- Single-objective continuous optimisation — Bayesian optimisation, simulated annealing, or domain-specific solvers usually beat GAs.

## Picker shortcut

| If you need… | Reach for… |
|--------------|-----------|
| Multi-objective EA in Java / research-replication | **MOEA Framework** (this section) |
| Multi-objective EA in Python (greenfield) | **pymoo** |
| Custom evolutionary primitives in Python | **DEAP** |
| Neuroevolution for RL / control | **NEAT-Python** / **PyNeat** / OpenAI ES code |
| Black-box continuous optimisation | **CMA-ES** (`cma` in Python) / `scipy.optimize.differential_evolution` |
| AutoML by evolution | **TPOT** / **EvolveAI-Zero** lineage |
| Constraint-handling industrial problems | **MOEA Framework** / **jMetal** / **pymoo** |

## Keywords

`#genetics` `#evolutionary` `#ga` `#moea` `#nsga` `#cma-es` `#neat` `#neuroevolution`

## See also

- [[../fundamentals/no_deep_learning_needed|no_deep_learning_needed]] — there is a *lot* of ML that doesn't need deep learning; evolutionary methods are part of that conversation.
- [[../fundamentals/pattern_learning|pattern_learning]] — the meta-topic landing.
- [[../frameworks/h2o|h2o]] — H2O's AutoML uses some GA-flavoured search internally.
- [[../README]] — ML section landing.
