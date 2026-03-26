# k-Point THC Factorization (Bloch Orbital THC)

> **Source:** Rubin, Berry, Malone, White, Khattar, DePrince, Sicolo, Kühn, Kaicher, Lee, Babbush, arXiv:2302.05531
> **Tags:** #trick #tensor-hypercontraction #periodic-systems #Bloch-orbitals #Coulomb-operator #density-fitting

## What it does

Extends the [[THC Non-Orthogonal Diagonal Coulomb Representation|tensor hypercontraction]] factorization of the two-electron integrals from molecules to periodic systems, by decomposing the cell-periodic part of Bloch orbital densities rather than the full orbitals. The result: the central tensor $\zeta_{\mu\nu}$ depends on momentum transfer $Q$ and reciprocal lattice vectors $G$ (at most $8^2$ distinct values) rather than on four independent crystal momenta.

## The trick

In the molecular THC factorization, the density is expanded over $M$ grid points:

$$\phi_p(\mathbf{r})\phi_q(\mathbf{r}) \approx \sum_\mu \xi_\mu(\mathbf{r})\, \phi_p(\mathbf{r}_\mu)\, \phi_q(\mathbf{r}_\mu)$$

For Bloch orbitals $\phi_{pk}(\mathbf{r}) = e^{i\mathbf{k}\cdot\mathbf{r}} u_{pk}(\mathbf{r})$, the key move is to factor the cell-periodic part:

$$u^*_{pk_p}(\mathbf{r})\, u_{qk_q}(\mathbf{r}) \approx \sum_\mu \xi_\mu(\mathbf{r})\, u^*_{pk_p}(\mathbf{r}_\mu)\, u_{qk_q}(\mathbf{r}_\mu)$$

This gives the two-electron integral as:

$$V_{pk, q(k\ominus Q), r(k'\ominus Q), sk'} = \sum_{\mu\nu} \chi^{(\mu)*}_{pk}\, \chi^{(\mu)}_{q(k\ominus Q)}\; \zeta^{Q, G_{k,k-Q}, G_{k',k'-Q}}_{\mu\nu}\; \chi^{(\nu)*}_{r(k'\ominus Q)}\, \chi^{(\nu)}_{sk'}$$

where $\chi^{(\mu)}_{qk} = u_{qk}(\mathbf{r}_\mu)$ are the cell-periodic parts evaluated at grid points, and the central tensor is:

$$\zeta^{Q, G_1, G_2}_{\mu\nu} = \int d\mathbf{r}\int d\mathbf{r}'\; e^{-i(Q+G_1)\cdot\mathbf{r}}\, \xi_\mu(\mathbf{r})\, V(\mathbf{r},\mathbf{r}')\, \xi_\nu(\mathbf{r}')\, e^{i(Q+G_2)\cdot\mathbf{r}'}$$

The $G$ vectors arise from the modular subtraction: $G_{k,k-Q} = (k - (k\ominus Q)) - Q$. For a Monkhorst-Pack grid, there are at most 8 distinct $G$ values (one per octant). So the central tensor has at most $8^2 \times N_k \times M^2$ entries — compared to $N_k^3 \times M^2$ if you naively enumerated all four-momentum combinations.

**THC factor optimization:** The ISDF (interpolative separable density fitting) approach provides initial THC factors, which are then reoptimized to minimize $\lambda$ via L1-regularized least-squares (as in [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]]). A rank parameter $c_{\rm THC} = 8$ (i.e., $M = 4N$) gives MP2 errors $< 0.1$ mHa/cell.

## When to reach for it

- Quantum simulation of periodic materials in atom-centered (Gaussian) orbital bases when [[THC Non-Orthogonal Diagonal Coulomb Representation|THC qubitization]] is desired
- When you need a compact factorization of the periodic Coulomb tensor that respects translational symmetry
- Classical electronic structure: the factorized form also reduces the classical cost of storing and manipulating the two-electron integrals

## Complexity

- Central tensor storage: $O(N_k M^2)$ — reduced from $O(N_k^3 M^2)$ by the $G$-vector structure
- Leaf tensor storage: $O(N_k N M)$ where $N$ is the number of bands per cell
- Classical THC factor optimization: the bottleneck — L1-regularized reoptimization scales prohibitively with $N_k$, limiting practical deployment
- Quantum block encoding: $O(N_k N)$ Toffolis per walk step (no asymptotic improvement from symmetry, due to unary iteration floor)

## Caveat

The classical cost of computing and optimizing the k-THC factors is the fatal limitation. ISDF provides a starting point, but the subsequent L1-regularized optimization to reduce $\lambda$ doesn't scale to large $N_k$. For the benchmark systems in the paper, only small k-meshes ($\leq 3 \times 3 \times 3$) were tractable. This means k-THC is currently impractical for the large-$N_k$ simulations needed to converge materials properties to the thermodynamic limit.

Also: $\lambda_{\rm THC}$ in the symmetry-adapted setting scales worse than in the supercell case (less compression freedom), partially negating the structural benefits.

## Related notes
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]] — the molecular version
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — molecular THC qubitization
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — L1-regularized THC optimization
- [[Symmetry-Adapted Block Encoding via QROAM Data Reduction]]
- [[Controlled Momentum-Register Swaps for Periodic Block Encodings]]
- [[QROM-Loaded Givens Rotation Networks]]
- [[Double Factorization of Two-Electron Integrals]]
