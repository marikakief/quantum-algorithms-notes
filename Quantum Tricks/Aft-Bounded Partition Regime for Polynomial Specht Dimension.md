# Aft-Bounded Partition Regime for Polynomial Specht Dimension

> **Source:** Panova, arXiv:2502.20253
> **Tags:** #trick #representation-theory #symmetric-group #dimension-bounds

## What it does

Detects when an $S_n$ irrep has only polynomial dimension by bounding the size outside its longest row or column.

## The trick

For a partition $\lambda\vdash n$, define
$$
\operatorname{aft}(\lambda)=n-\max\{\lambda_1,\ell(\lambda)\}.
$$
Because $f_\lambda=f_{\lambda'}$, one may transpose and assume $\lambda_1\ge \ell(\lambda)$, so $\operatorname{aft}(\lambda)=n-\lambda_1$.

If $\operatorname{aft}(\lambda)=k$, the number of standard Young tableaux satisfies
$$
\binom{n-k}{k}\le f_\lambda\le \frac{n^k}{\sqrt{k!}} .
$$
The upper bound comes from choosing the $k$ entries outside the first row and filling the remaining skew part by at most $\sqrt{k!}$ tableaux. The lower bound fixes a valid first-row prefix and varies a bounded tail.

The useful contrapositive is the algorithmic one: if $f_\lambda\le n^c$, then $\operatorname{aft}(\lambda)\le 4c^2$ for the regimes used in Panova's paper. Polynomial representation dimension means bounded tail.

## When to reach for it

Use this when a representation-theoretic algorithm has runtime depending on $f_\lambda$ and you need to know whether the polynomial-dimensional cases have extra combinatorial structure. It is especially useful for Kronecker and plethysm algorithms where a long first row makes otherwise large sums collapse.

## Complexity

Computing $\operatorname{aft}(\lambda)$ is linear in the partition encoding. The value of the trick is not the computation itself, but the reduction from a dimension promise $f_\lambda\le n^c$ to a bounded structural parameter $O(c^2)$.

## Caveat

The constants are coarse. This is a complexity-theory classifier, not a sharp asymptotic estimate for a fixed partition family.

## Related notes

- [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]]
- [[Bounded-Tail Kronecker Expansion via Jacobi-Trudi]]
- [[Plethysm Coefficient Extraction by Ordered-Composition Polytopes]]
