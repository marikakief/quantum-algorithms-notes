
> **Source:** Motta, Ye, McClean, Li, Minnich, Babbush, Chan, arXiv:1808.02625
> **Tags:** #trick #quantum-chemistry #Hamiltonian-decomposition #Cholesky-decomposition #low-rank #Trotter #circuit-compilation

## What it does

Reduces the $O(N^4)$ two-electron integral tensor $h_{pqrs}$ to a sum of $L = O(N)$ pairwise number–number interaction terms in rotated orbital bases, enabling Trotter steps with $O(N^2 \log N)$ gate complexity instead of $O(N^4)$.

## The trick

**First factorization:** Reshape $h_{pqrs}$ into an $N^2 \times N^2$ supermatrix $h_{ps,qr}$ (real symmetric by eightfold integral symmetry). Cholesky-decompose it:

$$h_{ps,qr} \approx \sum_{\ell=1}^{L} L^{(\ell)}_{ps}\, L^{(\ell)}_{qr}$$

Truncate: keep the smallest $L$ with $\|h - \sum_\ell L^{(\ell)} \otimes L^{(\ell)}\|_\infty < \varepsilon$. Empirically $L = O(N)$.

**Second factorization:** Each $L^{(\ell)}$ is an $N \times N$ real symmetric matrix. Diagonalize and truncate:

$$L^{(\ell)}_{ps} = \sum_{i=1}^{\rho_\ell} U^{(\ell)}_{pi}\, \lambda^{(\ell)}_i\, U^{(\ell)}_{si}$$

retaining eigenvalues with $\sum_{j > \rho_\ell} |\lambda^{(\ell)}_j| < \varepsilon$. Empirically $\langle \rho_\ell \rangle = O(\log N)$ for increasing molecule size.

The two-electron operator becomes:

$$V \approx \sum_{\ell=1}^{L} \sum_{ij} \frac{\lambda^{(\ell)}_i \lambda^{(\ell)}_j}{2}\, n^{(\ell)}_i\, n^{(\ell)}_j$$

where $n^{(\ell)}_i$ are number operators in the rotated basis $U^{(\ell)}$.

**Circuit:** Each term $V^{(\ell)}$ is a diagonal $n_i n_j$ interaction in basis $\ell$. Implement via:
1. [[Givens Rotation Slater Determinant Preparation|Givens rotation]] network for inter-basis rotation $\tilde{U}^{(\ell)}$: $O(N\rho_\ell)$ gates
2. [[Fermionic Swap Network]] for pairwise $n_i n_j$ evolution: $\binom{\rho_\ell}{2}$ gates, depth $\rho_\ell$

All on a linear nearest-neighbor architecture.

## When to reach for it

- Compiling Trotter steps for molecular Hamiltonians in arbitrary Gaussian/molecular orbital bases where the Coulomb tensor is dense
- When you want to stay in the molecular orbital basis (not switch to plane waves) but need better than $O(N^4)$ gate scaling
- Near-term Trotter-based simulation where circuit depth on a linear chain is the bottleneck
- As the Hamiltonian decomposition step in [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|unitary compression]] or ADAPT-VQE circuit compilation

## Complexity

- **Gate count per Trotter step:** $O(N^2 \log N)$ (increasing molecule size) or $O(N^3)$ (fixed molecule, increasing basis)
- **Circuit depth:** $O(N^2)$ on a linear chain
- **Classical preprocessing:** $O(N^4)$ for the Cholesky decomposition; $O(N^3 L)$ for eigenvalue decompositions

## Caveat

- The $L = O(N)$ and $\langle \rho_\ell \rangle = O(\log N)$ scalings are empirical, not rigorously proven. Pathological molecular geometries could in principle give worse ranks.
- For the [[Unitary Coupled Cluster (UCC) Ansatz|uCC operator]], the antisymmetry of amplitudes gives $L = O(N^2)$ with molecule size, so the double factorization advantage is weaker. [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Greedy unitary compression]] (Rubin, Lee, Babbush 2021) handles this better.
- Does not address Trotter error — only the per-step gate count. Truncation changes the commutator structure, so Trotter error analysis must account for $H' \neq H$.
- For fault-tolerant simulation, [[THC Non-Orthogonal Diagonal Coulomb Representation|tensor hypercontraction]] with [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] (Lee, Berry et al. 2021) now gives better total complexity.

## Related notes
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]]
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Fermionic Swap Network]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[Partial Basis Rotation (Reduced Givens Count)]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Repurposes this decomposition for measurement grouping rather than Trotter compilation
- [[Basis Rotation Grouping for VQE Measurement]]
