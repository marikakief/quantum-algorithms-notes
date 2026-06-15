# Random Quotient Sampling to Generate Hidden Cosets

## Pattern

Suppose an HSP instance over $G$ has a normal subgroup $M\triangleleft G$ such that recovering $HM/M$ is enough to finish the problem. If the index

$$
[G:HM]
$$

is polynomially bounded, sample random cosets from $G/M$ and keep those that pass a membership/intersection test for $HM/M$.

As long as the current generated subgroup $\langle T\rangle$ is not yet $HM/M$, a uniformly random coset of $G/M$ lands in $(HM/M)\setminus\langle T\rangle$ with inverse-polynomial probability. Adding such a coset at least doubles $|\langle T\rangle|$, so only $O(\log |G|)$ successful extensions are needed.

## Why it helps

Enumeration of $G/M$ may be too expensive even when $HM$ has small index in $G$. Random quotient sampling turns a search over all cosets into a coupon-collection-like process whose cost scales with $[G:HM]$, not $[G:M]$.

## Where it appears

- [[Quantum Solution to the HSP for Poly-Near-Hamiltonian Groups (Gavinsky 2004) — Paper Notes]] — algorithm `HS_2(G,d,\delta)` uses $O(nd\log(1/\delta))$ samples when $[G:HM_G]\leq d$.

## Reuse checklist

- Need uniform or near-uniform sampling in the quotient $G/M$.
- Need a test that decides whether a sampled coset contributes to $HM/M$.
- Need a final subroutine that solves the residual normal HSP once $HM/M$ is generated.
