
> **Tags:** #trick #optimization #remez #chebyshev #QSP
> **Source:** Low–Yoder–Chuang PRX 2016

## What it does
Designs near-optimal quantum response polynomials using classical digital signal processing tools.

## The trick
Map target gate-response problem to FIR filter approximation.

Then use:
- Remez/Parks–McClellan for minimax (equiripple) optimality
- least-squares for average-case objectives
- maximally-flat constructions for derivative cancellation near a point

After optimization, enforce realizability constraints and compile phases.

## Why it's useful
Huge performance gain over ad hoc pulse search; gives systematic optimality guarantees and bandwidth/error trade-offs.

## When to use
Whenever objective is "best approximation over interval" in [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]/composite-gate design.

## Caveat
Optimized polynomial still must pass parity + unitarity feasibility checks before phase compilation.

## Related Paper Notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
