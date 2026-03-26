# Full-Rank Ising Coupling via Numerical Basis Optimization

> **Source:** Rubin, Lee, Babbush, arXiv:2109.05010 (2021)
> **Tags:** #trick #operator-decomposition #circuit-compilation #information-density

## What it does

Replaces the rank-1 Ising coupling matrices $J_{pq} = \lambda_p \lambda_q$ (from analytical SVD/Takagi decompositions) with full-rank $J_{pq}$ matrices obtained by numerical optimization, dramatically increasing the information content per tensor factor in a sum-of-squares operator decomposition.

## The trick

In the standard sum-of-squares decomposition of a two-body operator, each term is:

$$e^{Z_l^2} = U_l \, e^{\sum_{pq} J^{(l)}_{pq} n_p n_q} \, U_l^\dagger$$

Analytical decompositions (SVD, Takagi) diagonalize a one-body operator whose eigenvalues $\lambda_p$ determine $J_{pq} = \lambda_p \lambda_q$. This outer-product structure means $J$ has rank 1 — it stores only $n$ independent parameters out of a possible $n(n+1)/2$.

The [[Greedy Sum-of-Squares Operator Decomposition|unitary compression]] algorithm doesn't constrain $J$ to be rank 1. The optimization directly maximizes the total $n_i n_j$ weight in a chosen basis, and the extracted $J_{pq}$ is whatever the rotated tensor's diagonal block happens to be. This typically has full rank, so each factor carries $O(n^2)$ parameters instead of $O(n)$.

**Compression consequence:** For [[Unitary Coupled Cluster (UCC) Ansatz|UCC doubles]] of HF in a minimal basis (12 spin-orbitals), Takagi needs ~40 factors for sub-mHa accuracy; unitary compression needs ~10. The $4\times$ improvement is roughly what you'd expect from going rank-1 → full-rank in terms of information per factor.

## When to reach for it

- Whenever you're implementing a sum-of-squares circuit and the analytical decomposition produces too many layers.
- As a conceptual check: if your decomposition has rank-1 $J$ matrices, you're probably wasting circuit depth. Consider numerical optimization.
- Applies to any setting where interleaved Gaussian + Ising circuits are the target structure (UCC, $k$-uCJ, Trotterized Hamiltonians in certain bases).

## Complexity

No additional circuit cost — the Ising interaction layer $e^{\sum_{pq} J_{pq} n_p n_q}$ costs the same whether $J$ is rank 1 or full rank ($O(n^2)$ gates via [[Fermionic Swap Network|swap network]]). The improvement is purely in the number of layers $L$ needed.

Classical cost: $O(n^5)$ per greedy iteration for the numerical optimization (see [[Greedy Sum-of-Squares Operator Decomposition]]).

## Caveat

Full-rank $J$ means the Ising layer is no longer a simple product of commuting two-body terms — it's a dense $n_p n_q$ interaction. This is fine for [[Fermionic Swap Network|swap network]] implementation (the swap network handles arbitrary pairwise $n_p n_q$ regardless of rank), but if you were hoping to further simplify the Ising layer, rank-1 structure would help and you've lost it.

## Related notes
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]]
- [[Greedy Sum-of-Squares Operator Decomposition]]
- [[Fermionic Swap Network]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
