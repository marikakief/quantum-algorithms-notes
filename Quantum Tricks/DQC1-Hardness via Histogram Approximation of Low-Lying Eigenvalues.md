# DQC1-Hardness via Histogram Approximation of Low-Lying Eigenvalues

> **Source:** Gyurik, Cade, Dunjko, arXiv:2005.02607 (2022)
> **Tags:** #trick #complexity #DQC1 #spectral-density

## What it does
Proves DQC1-hardness of spectral density estimation problems by reducing normalised sub-trace estimation to histogram construction using binned eigenvalue counts.

## The trick
Given oracle access to an LLSD (low-lying spectral density) estimator:

1. Partition the interval $[0, b]$ into bins of width $w$
2. For each bin $[jw, (j+1)w]$, estimate the eigenvalue count by taking the difference $N_H(0, (j+1)w) - N_H(0, jw)$
3. The difference gives the count in each bin, with eigenvalues misplaced by at most one bin due to imprecision at bin boundaries
4. Compute the mean of this histogram — it approximates the normalised sub-trace $\frac{1}{2^n}\sum_{k: \lambda_k \leq b} \lambda_k$
5. Since normalised sub-trace estimation is DQC1-hard (Brandão), LLSD is DQC1-hard

The one-bin misplacement doesn't affect the mean calculation enough to break the reduction (the error can be absorbed into the polynomial precision requirements).

## When to reach for it
- Proving classical hardness of eigenvalue-counting or spectral density problems
- Connecting new quantum algorithms to DQC1 to argue they resist dequantization
- When you need to reduce a "counting eigenvalues" problem to a "computing a spectral quantity" problem

## Complexity
The reduction uses $O(\text{poly}(n))$ non-adaptive queries to the LLSD oracle (polynomial-time truth-table reduction).

## Caveat
The reduction is to DQC1 hardness for *general* sparse PSD matrices, not specifically for combinatorial Laplacians. Whether combinatorial Laplacians of clique complexes are rich enough to encode DQC1-hard instances remains open.

## Related notes
- [[Towards Quantum Advantage via Topological Data Analysis (Gyurik-Cade-Dunjko 2022) — Paper Notes]]
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes]]
