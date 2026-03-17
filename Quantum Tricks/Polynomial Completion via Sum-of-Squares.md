# Polynomial Completion via Sum-of-Squares

> **Tags:** #trick #QSP #polynomials #synthesis
> **Source:** Low–Chuang PRL 2017 (+ companion QSP synthesis work)

## What it does
Given target [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] polynomial components (e.g., $A,C$), constructs compatible companion components so the full 2x2 transfer function is exactly unitary and phase-synthesizable.

## The trick
Enforce unitary constraint pointwise:
$$A(\theta)^2 + B(\theta)^2 + C(\theta)^2 + D(\theta)^2 = 1.$$

If only part is specified, complete the remainder using SOS/Fejér–Riesz-style factorization, then run [[Recursive Phase Extraction from Polynomial Coefficients|phase-angle synthesis]].

## Why it matters
Bridges approximation theory and executable [[QSVT Meta-Template|QSP circuits]]. Without completion, many good approximants are not directly realizable. See also [[SU(2) Response Tuple Characterization]] for the feasibility conditions this satisfies.

## When to use
Any time you derive polynomial approximants analytically and need guaranteed implementability.

## Related Paper Notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[SOSSA — Sum-of-Squares Spectral Amplification]]
