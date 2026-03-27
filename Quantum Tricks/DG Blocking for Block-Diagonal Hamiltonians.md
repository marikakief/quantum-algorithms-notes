
> **Source:** McClean, Faulstich, Zhu, O'Gorman, Qiu, White, Babbush, Lin, arXiv:1909.00028
> **Tags:** #trick #basis-set #block-diagonal #SVD #Hamiltonian-reduction #quantum-chemistry

## What it does

Compresses a set of delocalized active space orbitals (e.g., Gaussian molecular orbitals) into a smaller set of block-local functions, such that the two-electron integrals retain block-diagonal structure inherited from an underlying diagonal primitive basis.

## The trick

Start with two ingredients:
1. A **primitive basis** $\{\chi_\mu\}_{\mu=1}^{N_p}$ with diagonal two-body operator: $v_{\mu\sigma\gamma\nu} = v^{(p)}_{\mu\nu} \delta_{\mu\sigma} \delta_{\gamma\nu}$. Examples: [[Plane-Wave Dual Basis|plane-wave dual functions]], Gausslets.
2. An **active space** $\{\varphi_p\}_{p=1}^{N_a}$ expressed in the primitive basis via $\Phi \in \mathbb{C}^{N_p \times N_a}$ with orthogonal columns.

Partition the primitive indices into $N_b$ blocks (one per atom, typically). For each block $\kappa$, take the submatrix $\Phi_\kappa$ and perform a truncated SVD:

$$\Phi_\kappa \approx U_\kappa S_\kappa V^\dagger_\kappa$$

retaining singular values above tolerance $\tau$. The DG basis functions are:

$$\phi_{\kappa,j}(r) = \sum_{\mu \in \kappa} \chi_\mu(r) (U_\kappa)_{\mu,j}$$

Because $U = \text{diag}[U_1, \ldots, U_{N_b}]$ is block-diagonal, the two-body tensor satisfies:

$$v_{\kappa,i;\kappa',i';\lambda,j;\lambda',j'} = v^{(d)}_{\kappa,\kappa';i,i',j,j'} \delta_{\kappa\lambda} \delta_{\kappa'\lambda'}$$

This gives $O(N_b^2 n_\kappa^4)$ two-body terms instead of $O(N_a^4)$. Since $n_\kappa$ (functions per block) converges to a constant with system size, the effective scaling is $O(N_d^2)$.

The one-body operator can remain fully dense — the block structure only constrains the two-body terms, which dominate algorithmic cost.

## When to reach for it

- You have a quantum chemistry Hamiltonian in a molecular orbital basis with $O(N^4)$ integrals, and want to reduce the integral count for [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]], [[Truncated Taylor Series Simulation|LCU]], or [[Fermionic Swap Network|swap network]]-based simulation.
- You want to use DMRG but your active space orbitals are too delocalized for efficient tensor-network contraction. The DG basis restores spatial locality.
- You need a systematic way to interpolate between compact (MO) and sparse (diagonal) Hamiltonian representations, controlled by the SVD tolerance $\tau$.

## Complexity

- Classical preprocessing: $O(N_b \cdot N_a^2 \cdot n_\kappa)$ for the SVD steps (dominated by forming and factoring each $\Phi_\kappa$).
- Resulting Hamiltonian: $O(N_b^2 n_\kappa^4)$ two-body integrals.
- $n_\kappa \sim 10$–$23$ per atom empirically (hydrogen chains, cc-pVDZ active space, plane-wave dual primitive).

## Caveat

The convergence of $n_\kappa$ to a constant is empirically demonstrated only on hydrogen chains (1D geometry). More complex molecular geometries — especially 3D structures with transition metals or irregular coordination — may require larger blocks, weakening the advantage. The SVD tolerance $\tau$ has no rigorous relationship to the resulting energy error, though empirical results show insensitivity to even aggressive truncation.

## Related notes
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]]
- [[Plane-Wave Dual Basis]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Fermionic Swap Network]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
