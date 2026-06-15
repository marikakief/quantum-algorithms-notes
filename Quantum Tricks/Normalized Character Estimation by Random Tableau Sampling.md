# Normalized Character Estimation by Random Tableau Sampling

> **Source:** Stephen P. Jordan, arXiv:0811.0562
> **Tags:** #trick #representation-theory #symmetric-group #classical-randomized-algorithms

## What it does

Estimates normalized $S_n$ irreducible characters additively by sampling standard Young tableaux.

## The trick

For a partition $\lambda\vdash n$ and conjugacy class $\mu\vdash n$, Roichman's formula writes the character as
$$
\chi^\lambda_\mu=\sum_\Lambda W_\mu(\Lambda),
$$
where the sum is over standard Young tableaux $\Lambda$ of shape $\lambda$. The weight $W_\mu(\Lambda)$ is a product of local factors in $\{-1,0,1\}$ determined by relative positions of consecutive entries in $\Lambda$, and can be computed in polynomial time.

Since $d_\lambda$ is exactly the number of standard Young tableaux of shape $\lambda$,
$$
\frac{\chi^\lambda_\mu}{d_\lambda}=\mathbb E_{\Lambda\sim \mathrm{Unif}(\lambda)}[W_\mu(\Lambda)].
$$
Use the Greene--Nijenhuis--Wilf hook-walk algorithm to sample $\Lambda$ uniformly, compute $W_\mu(\Lambda)$, and average.

## When to reach for it

- A trace or normalized character is enough; individual matrix elements are not needed.
- A character formula is a sum over exponentially many combinatorial basis objects, but those objects can be sampled uniformly.
- You want to distinguish whether a quantum matrix-element algorithm is solving a genuinely harder task than trace estimation.

## Complexity

The hook-walk sampler runs in $O(n^2)$ time per tableau sample, and $W_\mu(\Lambda)$ is polynomial-time computable. Since $|W_\mu(\Lambda)|\le1$, standard sampling gives additive error $\epsilon$ with $O(1/\epsilon^2)$ samples, up to logarithmic confidence factors.

## Caveat

This estimates the normalized character, not the raw character. It also does not give individual matrix elements. The distinction matters: in Jordan's paper, characters are classically easy to additive precision while individual matrix elements remain plausible quantum-speedup candidates.

## Related notes

- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
- [[Young--Yamanouchi Adjacent-Transposition Blocks]]
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes]]
