# Fourier-Series Entropy Estimation for Variational Gibbs States

> **Source:** Chowdhury, Low, Wiebe, arXiv:2002.00055
> **Tags:** #trick #entropy #gibbs-state #variational #fourier

## What it does
Approximates the entropy term $S(\rho) = -\operatorname{Tr}(\rho \log \rho)$ by replacing $\log$ with a truncated trigonometric series.

## The trick
Approximate $\log x$ on the relevant spectral interval by a finite Fourier or trigonometric expansion,

$$
\log x \approx a_0 + \sum_{m=1}^M \big(a_m \cos(mx) + b_m \sin(mx)\big).
$$

Substitute this into $S(\rho)$ so the entropy becomes a finite weighted sum of simpler observables. Estimate each term separately on the quantum device and combine them classically.

## When to reach for it
When the objective contains matrix entropy or another hard spectral function that is awkward to measure directly, but can be approximated well by a short basis expansion.

## Complexity
Uses $M$ measurement subroutines for a truncation order $M$, plus the usual sampling overhead for each term.

## Caveat
Works best when the relevant eigenvalue interval is controlled. Poor spectral conditioning or too-short truncation can bias the entropy badly.

## Related notes
- [[A Variational Quantum Algorithm for Preparing Quantum Gibbs States (Chowdhury-Low-Wiebe 2020) — Paper Notes]]
- [[Variational Quantum Eigensolver (VQE)]]
