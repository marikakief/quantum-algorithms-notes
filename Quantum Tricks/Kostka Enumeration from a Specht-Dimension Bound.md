# Kostka Enumeration from a Specht-Dimension Bound

> **Source:** Larocca--Havlicek, arXiv:2407.17649; Fayers (2019)
> **Tags:** #trick #classical-algorithm #representation-theory #young-tableaux

## What it does

Turns a small Specht-module dimension bound into a polynomial-time enumeration algorithm for Kostka numbers.

## The trick

The Kostka number $K^\mu_\lambda$ counts semistandard Young tableaux of shape $\lambda$ and content $\mu$. Fayers' monotonicity result implies that for every content $\mu$,
$$
K^\mu_\lambda \leq K^{(1^n)}_\lambda = d_\lambda,
$$
where $d_\lambda$ is the dimension of the $S_n$ Specht module labelled by $\lambda$.

So if $d_\lambda\leq \operatorname{poly}(n)$, direct enumeration is polynomial-width:

1. Start with the empty diagram of shape $\lambda$ and remaining content $\mu$.
2. Repeatedly choose a removable corner in the partially filled tableau.
3. Place the largest remaining symbol allowed by the content.
4. Keep only partial fillings that still satisfy row weak increase and column strict increase.
5. Deduplicate partial states.

Every completed state is a valid semistandard Young tableau, and every valid semistandard Young tableau has a reverse path through this construction. The number of live states at each depth is at most $d_\lambda$, and there are $n$ depths.

## When to reach for it

Use this when a quantum multiplicity algorithm is efficient only because an $S_n$ irrep dimension is small. Before claiming a quantum speedup, check whether the same small dimension bounds the number of combinatorial witnesses.

## Complexity

If $d_\lambda\leq \operatorname{poly}(n)$, the enumeration runs in polynomial time in $n$. The hook-length formula computes $d_\lambda$ in $O(n^2)$ arithmetic operations.

## Caveat

This works because Kostka numbers have a positive tableau model and the monotonicity bound controls the number of tableaux. It does not automatically extend to Kronecker or general plethysm coefficients, where no comparably simple positive combinatorial rule is known.

## Related notes

- [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes]]
- [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]]
- [[Dimension-Ratio Triage for Multiplicity Algorithms]]
