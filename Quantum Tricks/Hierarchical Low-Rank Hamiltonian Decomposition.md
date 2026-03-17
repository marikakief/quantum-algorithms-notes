# Hierarchical Low-Rank Hamiltonian Decomposition

> **Tags:** #trick #low-rank #hamiltonian-simulation #block-encoding
> **Source:** Low, Su, Tong, Tran, *On the complexity of implementing Trotter steps*, arXiv:2211.09133, PRX Quantum 4, 020323 (2023)

## What it does

When blocks of Hamiltonian coefficients (e.g., interaction matrices between spatial regions) have low numerical rank $\rho$, compresses the Hamiltonian so that Trotter steps cost proportional to $\rho$ rather than the raw block size.

## The trick

Partition the Hamiltonian into blocks (e.g., by spatial region, or by interaction type). Truncate each block's SVD at rank $\rho$ — discarding singular values below a threshold. Simulate the resulting compressed Hamiltonian via [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] of each low-rank factor.

For each rank-$\rho$ block $H_{\text{block}} \approx U \Sigma V^\dagger$ (truncated SVD), the simulation cost scales as $O(\rho \cdot \text{block dimension} \cdot t \cdot \mathrm{polylog})$ rather than $O(\text{block dimension}^2 \cdot t)$.

The hierarchical part: apply this recursively at multiple scales, so the total cost is a sum over scales rather than a product.

## When to reach for it

- Coulomb-like kernels (molecules, condensed matter) where off-diagonal blocks are well approximated by low-rank matrices.
- Double factorisation and related chemistry decompositions fall into this category.
- Already using [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] / [[Qubitization Iterate|qubitization]] infrastructure.

## Complexity

Favorable regimes approach near-$\rho n t\,\mathrm{polylog}$ behavior per Trotter step. The paper gives explicit bounds including a master theorem for the recursive algorithm. If effective rank $\rho$ grows with $n$, gains collapse.

## Relation to other decompositions

Double factorisation (Babbush et al.) and tensor hypercontraction are special cases of low-rank Hamiltonian decomposition. This paper's contribution is integrating the low-rank structure into the Trotter step implementation directly, rather than using it for [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] normalization alone.

## Caveat

Truncation at rank $\rho$ introduces an approximation error that must be controlled — the truncated Hamiltonian must be close enough to the original. For smooth (physically motivated) kernels, this error is usually small, but it needs to be accounted for in the overall error budget.

## Related Paper Notes
- [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Recursive Interval Decomposition for Power-Law Hamiltonians]]
