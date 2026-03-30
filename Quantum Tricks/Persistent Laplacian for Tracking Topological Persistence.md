# Persistent Laplacian for Tracking Topological Persistence

> **Source:** Hayakawa, arXiv:2111.00433 (2022); Wang-Nguyen-Wei (2020)
> **Tags:** #trick #persistent-homology #Laplacian #tda

## What it does
Characterises persistent Betti numbers — which count holes that survive across filtration steps — as the nullity of a single operator, the persistent Laplacian.

## The trick
For a simplicial pair $K \hookrightarrow L$ ($K \subseteq L$), define the persistent Laplacian:

$$\Delta_q^{K,L} = \partial_{q+1}^{L,K} (\partial_{q+1}^{L,K})^* + (\partial_q^K)^* \partial_q^K$$

where $\partial_{q+1}^{L,K}$ is the boundary operator of $L$ restricted so its image lies in the chain space of $K$. Then:

$$\beta_q^{K,L} = \text{nullity}(\Delta_q^{K,L})$$

This converts the algebraic-topological question "how many holes survive from $K$ to $L$?" into a spectral question "what is the kernel dimension of $\Delta_q^{K,L}$?" — making it amenable to quantum algorithms via [[Kernel Dimension Estimation via QPE on Mixed States|kernel estimation]].

The persistent Laplacian's "up" part $\Delta_{q,\text{up}}^{K,L}$ can be computed as a Schur complement of the ordinary $L$-Laplacian, avoiding explicit construction of the persistent boundary operator.

## When to reach for it
- Computing persistent Betti numbers quantumly or classically
- Any setting where you need a spectral characterisation of topological persistence
- Designing quantum algorithms for persistent homology, persistence diagrams, or barcodes

## Complexity
The persistent Laplacian has the same sparsity as the ordinary Laplacian ($O(n^2)$-sparse), so simulation costs are comparable. The additional cost comes from the Schur complement construction (pseudo-inverse via QSVT).

## Caveat
Requires a spectral gap $\lambda_{\min}^q$ for the persistent Laplacian AND a gap $\gamma_{\min}^q$ for the Schur complement submatrix. The second condition is new relative to ordinary Betti number estimation and less well understood.

## Related notes
- [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes]]
- [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]] — Uses the combinatorial Laplacian (not persistent Laplacian) but addresses the related persistence question via harmonic overlap
- [[Schur Complement Block-Encoding for Projected Operators]]
- [[Kernel Overlap as BQP-Complete Primitive]]
