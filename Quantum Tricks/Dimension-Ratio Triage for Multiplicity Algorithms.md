# Dimension-Ratio Triage for Multiplicity Algorithms

> **Source:** Larocca--Havlicek, arXiv:2407.17649
> **Tags:** #trick #representation-theory #complexity #triage

## What it does

Tests whether a Fourier-sampling multiplicity algorithm is even plausibly efficient before worrying about circuit details.

## The trick

Many Fourier-sampling algorithms for representation multiplicities produce an outcome probability of the form
$$
\Pr[\text{target label}] = m\cdot {d_{\mathrm{small}}\over d_{\mathrm{large}}},
$$
where $m$ is the integer multiplicity. To recover $m$ exactly, one must estimate the probability to additive precision roughly
$$
{d_{\mathrm{small}}\over 2d_{\mathrm{large}}}.
$$
This forces
$$
N=O\!\left(\left({d_{\mathrm{large}}\over d_{\mathrm{small}}}\right)^2\right)
$$
samples. So the first check is not whether the relevant QFT is efficient; it is whether the dimension ratio is polynomial.

For Larocca--Havlicek's four cases, the ratios are:

| Problem | Ratio controlling sample cost |
|---|---|
| Kostka $K^\mu_\nu$ | $d_\nu$ |
| Littlewood--Richardson $c^\nu_{\lambda\mu}$ | $d_\nu/(d_\lambda d_\mu)$ |
| Plethysm $a^\nu_{\lambda\mu}$ | $d_\nu/(d_\lambda^d d_\mu)$ |
| Kronecker $g_{\lambda\mu\nu}$ | $d_\lambda d_\mu/d_\nu$ |

## When to reach for it

Use this as a fast sanity check for any proposed quantum algorithm that converts a representation-theoretic coefficient into a Fourier-label probability. It is especially useful for symmetric-group problems, where dimensions can be computed from the hook-length formula in $O(n^2)$ time.

## Complexity

The triage itself costs the time to compute the relevant irrep dimensions. For $S_n$ partitions, the hook-length formula gives $d_\lambda$ in $O(n^2)$ arithmetic operations, with large-integer arithmetic if the exact value is needed.

## Caveat

A good dimension ratio is necessary, not sufficient. You still need efficient QFTs, efficient state preparation, and no classical algorithm that exploits the same low-dimensional regime. The Kostka and Kronecker examples show the danger: small dimensions often also make classical enumeration or dynamic programming easier.

## Related notes

- [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes]]
- [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]]
- [[Multiplicity Estimation by Restricted-Isotypic Fourier Sampling]]
- [[Kostka Enumeration from a Specht-Dimension Bound]]
