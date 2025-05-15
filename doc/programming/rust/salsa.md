# Salsa

https://github.com/salsa-rs/salsa?tab=readme-ov-file


A generic framework for on-demand, incrementalized computation.

## Key idea
The key idea of salsa is that you define your program as a set of queries. Every query is used like function K -> V that maps from some key of type K to a value of type V. Queries come in two basic varieties:

- Inputs: the base inputs to your system. You can change these whenever you like.
- Functions: pure functions (no side effects) that transform your inputs into other values. The results of queries are memoized to avoid recomputing them a lot. When you make changes to the inputs, we'll figure out (fairly intelligently) when we can re-use these memoized values and when we have to recompute them.


https://salsa-rs.github.io/salsa/overview.html


https://salsa-rs.github.io/salsa/tutorial/ir.html


