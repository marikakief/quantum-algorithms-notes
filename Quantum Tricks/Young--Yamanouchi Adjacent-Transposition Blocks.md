# Young--Yamanouchi Adjacent-Transposition Blocks

> **Source:** Stephen P. Jordan, arXiv:0811.0562
> **Tags:** #trick #representation-theory #symmetric-group #quantum-circuits

## What it does

Implements symmetric-group irreps by reducing every permutation to adjacent swaps with $1\times1$ and $2\times2$ representation blocks.

## The trick

Use the Young--Yamanouchi basis, where basis states are standard Young tableaux of a fixed shape $\lambda\vdash n$. The adjacent transposition $\sigma_i=(i,i+1)$ acts by a local tableau rule:
$$
\rho_\lambda(\sigma_i)\Lambda
=\frac{1}{\tau_i^\Lambda}\Lambda
+\sqrt{1-\frac{1}{(\tau_i^\Lambda)^2}}\,\Lambda',
$$
where $\Lambda'$ swaps boxes $i$ and $i+1$, and $\tau_i^\Lambda$ is the axial distance from box $i+1$ to box $i$.

If $\Lambda'$ is nonstandard, then $\tau_i^\Lambda=\pm1$ and the coefficient of the invalid tableau is zero. So each adjacent transposition decomposes into independent $1\times1$ and $2\times2$ blocks. A permutation $\pi\in S_n$ can be decomposed by bubble sort into $O(n^2)$ adjacent transpositions, and the represented unitary is the product of the corresponding block unitaries.

## When to reach for it

- You need explicit quantum access to $S_n$ irreps in the standard Young-tableau basis.
- A nonabelian QFT is overkill, or you need a construction that extends to $A_n$.
- A representation has generator actions that are local moves on combinatorial basis states.

## Complexity

Each adjacent transposition has row sparsity at most $2$ and computable entries. A full permutation uses $O(n^2)$ adjacent swaps, before Hadamard-test sampling. Jordan's matrix-element estimation runs in $\operatorname{poly}(n,1/\epsilon)$ time.

## Caveat

This is basis-specific and gives additive estimates. For typical matrix elements in exponentially large irreps, $1/\operatorname{poly}(n)$ additive precision may still be too coarse to reveal anything useful.

## Related notes

- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
- [[Irrep Extraction by Fourier-Conjugating the Regular Representation]]
- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]]
