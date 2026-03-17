# Partial-Quadrature Completion via Root Grouping

> **Tags:** #trick #QSP-precursor #sum-of-squares #root-finding
> **Source:** Low–Yoder–Chuang PRX 2016

## What it does
Completes unspecified response quadratures (e.g., given only A,C) into a full unitary-consistent polynomial tuple.

## The trick
1. Form residual polynomial from unitarity constraint ("what is left" after specified quadratures).
2. Use variable transform/symmetry structure (e.g., Weierstrass substitution) to expose palindromic root patterns.
3. Group roots by conjugation/symmetry classes.
4. Factor residual as sum of two squares, recovering missing quadratures.

This turns partial objective design into a realizable full [[SU(2) Response Tuple Characterization|SU(2) response]].

## When to use
- You optimize only fidelity/transition amplitude (partial specs)
- Need guaranteed realizability before phase extraction (see [[Recursive Phase Extraction from Polynomial Coefficients]])

## Caveat
Numerical root-finding quality controls overall synthesis stability.

## Related Paper Notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[SOSSA — Sum-of-Squares Spectral Amplification]]
