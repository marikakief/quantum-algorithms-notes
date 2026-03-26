> **Source:** Jarrod R. McClean, Fabian M. Faulstich, Qinyi Zhu, Bryan O'Gorman, Yiheng Qiu, Steven R. White, Ryan Babbush, Lin Lin, *Discontinuous Galerkin discretization for quantum simulation of chemistry*, arXiv:1909.00028, New J. Phys. **22**, 093015 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/1909.00028) · [Journal](https://doi.org/10.1088/1367-2630/ab9d9f)
> **Tags:** #quantum-simulation #quantum-chemistry #basis-set #discontinuous-Galerkin #block-diagonal #Gausslet #plane-wave-dual #fermionic-swap-network #LCU #qubitization #DMRG #fault-tolerant

---

## The computational problem

Choose a discretization (basis set) for the electronic structure Hamiltonian

$$\hat{H} = -\sum_i \frac{\nabla^2_{r_i}}{2} - \sum_{I,j} \frac{Z_I}{|R_I - r_j|} + \sum_{i < j} \frac{1}{|r_i - r_j|} + E_{II}$$

that balances two competing concerns:

1. **Compactness:** Gaussian/molecular orbital (MO) bases use few functions ($N_a$) but produce $O(N_a^4)$ two-electron integrals, with fully dense structure.
2. **Sparsity:** Primitive diagonal bases ([[Plane-Wave Dual Basis|plane-wave dual]], Gausslets) diagonalize the two-body operator to $O(N_p^2)$ terms, but require many more functions ($N_p \gg N_a$) for equivalent accuracy.

The question: can we systematically interpolate between these regimes — use fewer basis functions than the primitive basis while keeping a (block-)diagonal Hamiltonian structure?

## What the paper does

Introduces a **discontinuous Galerkin (DG) blocking procedure** that takes any primitive basis with diagonal Coulomb interactions and an active space defined by delocalized orbitals, then constructs a compressed basis where the two-electron integrals have **block-diagonal** structure. The DG basis uses $N_d$ functions partitioned into $N_b$ blocks of size $n_\kappa$, with $N_d = \sum_\kappa n_\kappa$. The two-body operator scales as $O(N_b^2 n_\kappa^4)$; since $n_\kappa$ converges to a constant with system size, the effective scaling is $O(N_d^2)$.

The paper demonstrates:
- A scaling crossover from $O(N_h^{4.5})$ (Gaussian MO) to $O(N_h^{2.6})$ (DG) for fault-tolerant quantum simulation cost on hydrogen chains, with constant-factor crossover at **15–20 atoms**.
- That correlated calculations (CCSD, DMRG) in the DG basis maintain the accuracy of the original active space.
- A hybrid active space approach combining UHF (static correlation, in Gausslets) with weighted Gaussian basis (dynamic correlation) gives DMRG results near the complete basis set limit with 1–2 orders of magnitude speedup.

My assessment: this is an enabling methodological paper rather than a new algorithm. The DG basis doesn't change the asymptotic gate complexity formulas — it changes which regime you're in (effectively turning $N_a^4$ into $N_d^2$ for the integral count). The crossover at 15–20 atoms is right at the boundary of what matters for quantum advantage. The real win may be for classical tensor network methods (DMRG), where the spatial locality of DG functions directly reduces bond dimension. For quantum computing, the benefit flows through the $\lambda$ factor and integral count reduction in [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] and [[Truncated Taylor Series Simulation|LCU]] methods.

## The algorithm / construction

### Step 1: Start with a diagonal primitive basis

Choose a primitive basis $\{\chi_\mu(r)\}_{\mu=1}^{N_p}$ with the diagonal property:

$$v_{\mu\sigma\gamma\nu} \to v^{(p)}_{\mu\nu} \delta_{\mu\sigma} \delta_{\gamma\nu}$$

The two-body operator in this basis is:

$$\hat{H}^{(p)} = \sum_{\mu,\nu} h^{(p)}_{\mu\nu} \hat{b}^\dagger_\mu \hat{b}_\nu + \frac{1}{2} \sum_{\mu,\nu} v^{(p)}_{\mu\nu} \hat{n}_\mu \hat{n}_\nu$$

with only $O(N_p^2)$ terms. Candidates include:
- **[[Plane-Wave Dual Basis]]:** Periodic sinc functions / DVR. Uniform resolution, long tails, $\sim 3000$ functions per atom. Best for periodic systems.
- **Gausslets:** Wavelet-transformed Gaussians. Variable resolution (more near nuclei), strict localization, delta-function-like integration. Better for molecules and tensor networks.

### Step 2: Define the active space

Choose $N_a$ orthonormal active space orbitals $\{\varphi_p(r)\}_{p=1}^{N_a}$ (Hartree-Fock, natural orbitals, Gaussian basis functions like cc-pVDZ). Express them in the primitive basis:

$$\varphi_p(r) = \sum_\mu \chi_\mu(r) \Phi_{\mu p}$$

where $\Phi \in \mathbb{C}^{N_p \times N_a}$ has orthogonal columns. The active space Hamiltonian has the standard quartic form with $O(N_a^4)$ two-electron integrals.

### Step 3: DG blocking via SVD

Partition the primitive index set $\Omega = \{1, \ldots, N_p\}$ into $N_b$ non-overlapping blocks $\mathcal{K} = \{\kappa_1, \ldots, \kappa_{N_b}\}$, typically one block per atom.

For each block $\kappa$, extract the submatrix $\Phi_\kappa = [\Phi_{\mu p}]_{\mu \in \kappa}$ and perform a truncated SVD:

$$\Phi_\kappa \approx U_\kappa S_\kappa V^\dagger_\kappa$$

retaining the $n_\kappa$ leading singular values above tolerance $\tau$. The DG basis functions for block $\kappa$ are:

$$\phi_{\kappa,j}(r) = \sum_{\mu \in \kappa} \chi_\mu(r) (U_\kappa)_{\mu,j}$$

Each DG function is a linear combination of primitive functions from a single block — still technically continuous, but with support (in the grid/index sense) restricted to one block. The block-diagonal structure of $U = \text{diag}[U_1, \ldots, U_{N_b}]$ ensures the two-body operator inherits block-diagonal structure:

$$v_{\kappa,i;\kappa',i';\lambda,j;\lambda',j'} = v^{(d)}_{\kappa,\kappa';i,i',j,j'} \delta_{\kappa\lambda} \delta_{\kappa'\lambda'}$$

The one-body operator $h^{(d)}$ can be fully dense — only the two-body structure matters.

### The block-diagonal Hamiltonian

$$\hat{H}^{(d)} = \sum_{\kappa,\kappa';j,j'} h^{(d)}_{\kappa,\kappa';j,j'} \hat{c}^\dagger_{\kappa,j} \hat{c}_{\kappa',j'} + \frac{1}{2} \sum_{\kappa,\kappa';i,i',j,j'} v^{(d)}_{\kappa,\kappa';i,i',j,j'} \hat{c}^\dagger_{\kappa,i} \hat{c}^\dagger_{\kappa',i'} \hat{c}_{\kappa',j'} \hat{c}_{\kappa,j}$$

The two-body operator has $O(N_b^2 n_\kappa^4)$ non-zero elements. Since $n_\kappa$ converges to a constant with increasing system size (empirically $\sim 10$–$23$ functions per atom depending on tolerance), the effective scaling is $O(N_d^2)$.

### Hybrid active space

For high-accuracy DMRG, the paper combines:
1. UHF density matrix $D_{\text{UHF}}$ from a Gausslet calculation (captures static correlation near the CBS limit)
2. Gaussian cc-pVDZ density matrix $D_{\text{Gaussian}}$ (captures empirically optimized dynamic correlation features)

Form $D = D_{\text{UHF}} + \alpha D_{\text{Gaussian}}$ with $\alpha \approx 0.01$, then define $\Phi$ from the leading natural orbitals of $D$. Apply the DG blocking procedure. This gives a basis that preserves both types of correlation while maintaining block-diagonal structure.

## Key results

### Scaling crossover (hydrogen chains, plane-wave dual primitive)

| Representation | Two-electron integral scaling | $\lambda$ scaling | Empirical LCU cost $\sim \sqrt{L}\lambda t$ |
|---|---|---|---|
| Gaussian (MO) | $O(N_h^{4.14})$ | $O(N_h^{2.52})$ | $O(N_h^{4.5} t)$ |
| DG ($\tau = 10^{-2}$) | $O(N_h^{2.18})$ | $O(N_h^{1.47})$ | $O(N_h^{2.6} t)$ |
| DG ($\tau = 10^{-1}$) | $O(N_h^{2.03})$ | $O(N_h^{1.42})$ | $O(N_h^{2.4} t)$ |
| Primitive (plane-wave dual) | $O(N_h^{1.54})$ | $O(N_h^{1.07})$ | $O(N_h^{1.8} t)$ |

The constant-factor crossover (where DG becomes cheaper than MO in absolute terms) occurs at **15–20 hydrogen atoms** for both $L$ and $\lambda$, depending on SVD tolerance and bond length.

### Block size convergence

The average number of DG functions per atom $\langle n_\kappa \rangle$ converges to a constant as system size grows (for fixed tolerance):
- $\tau = 10^{-1}$: $\langle n_\kappa \rangle \approx 14$
- $\tau = 10^{-2}$: $\langle n_\kappa \rangle \approx 17$
- $\tau = 10^{-3}$: $\langle n_\kappa \rangle \approx 23$

This convergence is the mechanism behind the improved scaling — the block size is a constant that doesn't grow with system size.

### Correlated accuracy (Gausslet primitive)

CCSD calculations in the DG basis match the active space CCSD energies to high precision across the entire potential energy surface for H$_2$ through H$_8$. The block-diagonal restriction does not introduce additional error beyond the active space approximation itself.

### DMRG with hybrid active space (H$_{10}$)

The DG + hybrid approach (7 UHF functions + cc-pVDZ features per block, 150 total functions) achieves near-CBS accuracy with 1–2 orders of magnitude reduction in DMRG computational cost compared to:
- Pure Gausslet primitive ($\sim 10{,}000$ functions)
- Pure Gaussian basis (dense integrals, high bond dimension)

The cost reduction comes from two sources: far fewer basis functions than the primitive, and spatial locality (bounded bond dimension from block structure).

## Quantum simulation with DG basis

### Swap networks for block-diagonal Hamiltonians

The [[Fermionic Swap Network|fermionic swap network]] of Kivlichan et al. is extended to exploit block-diagonal structure. The circuit has four stages:

1. **Intra-block, same spin:** 4-complete swap network $K^4_{n_\kappa/2}$ within each half-block. Depth $O(n_\kappa^3)$.
2. **Intra-block, mixed spin:** Double bipartite swap network on each block. Depth $O(n_\kappa^3)$.
3. **Permutation:** Reorder within each block in $O(n_\kappa)$ depth.
4. **Inter-block:** $N_b$ alternating layers of balanced double bipartite swap networks between adjacent block pairs. Depth $O(N_b n_\kappa^3)$.

Overall Trotter step depth: $O(N_b n_\kappa^3) = O(N_d n_\kappa^2)$. This interpolates between the $O(N_p)$-depth diagonal case ($n_\kappa = 1$) and the $O(N_a^3)$-depth fully general case ($N_b = 1$).

### LCU / qubitization approaches

For [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] and [[Truncated Taylor Series Simulation|Taylor series]] methods, the DG basis affects cost through:

- **$\lambda$ reduction:** DG $\lambda$ values are smaller than MO $\lambda$ values. Empirically, $\lambda_{\text{DG}} \propto N_h^{1.47}$ vs $\lambda_{\text{MO}} \propto N_h^{2.52}$.
- **PREPARE cost reduction:** Number of unique integrals $L = O(N_b^2 n_\kappa^4)$ vs $O(N_a^4)$. Using [[QROM (Quantum Read-Only Memory)|QROM]], this gives $O(\sqrt{L})$ T complexity with ancilla, or $O(L/N_d)$ without.
- **SELECT cost:** Unchanged relative to standard approaches.

Straightforward extension of qubitization (Berry et al. 2019, arXiv:1902.02134) to DG basis gives T complexity $O(n_\kappa^2 N_b \lambda t)$ per step. Additional savings possible by exploiting symmetries in the block structure.

### Alternative: low-rank factorization in DG basis

The [[Double Factorization of Two-Electron Integrals|double factorization]] approach can be applied within each $(\kappa, \kappa')$ block of the DG two-electron tensor. Since each block has bounded dimension $O(n_\kappa^2)$:
- Cholesky rank per block is bounded by $O(n_\kappa^2)$ in the worst case, $O(n_\kappa)$ empirically.
- Each $R_{\kappa\kappa'}$ factor can be implemented in constant depth for large systems.
- Overall Trotter depth: $O(N_b)$ using a lifted swap network at the block level.

## Comparison with prior work

| Approach | Basis functions | Two-body terms | LCU cost scaling | Locality |
|---|---|---|---|---|
| Gaussian / MO | $N_a$ (compact) | $O(N_a^4)$ | $O(N_a^{4.5} t)$ empirical | Delocalized |
| [[Plane-Wave Dual Basis\|Plane-wave dual]] | $N_p \gg N_a$ | $O(N_p^2)$ | $O(N_p^{1.8} t)$ empirical | Long tails |
| Gausslets | $N_p$ (variable) | $O(N_p^2)$ | $O(N_p^2)$ terms | Strictly local |
| **DG (this paper)** | $N_d$ ($N_a < N_d \ll N_p$) | $O(N_d^2)$ | $O(N_d^{2.6} t)$ empirical | Block-local |

The DG basis sits between the MO and primitive extremes: more functions than MO ($\sim 15$–$20$ per atom vs $\sim 5$ for cc-pVDZ), but with block-diagonal two-body integrals. The crossover at 15–20 atoms makes DG relevant for exactly the system sizes where quantum advantage is expected to begin.

## Limits / caveats

1. **Block size is "constant" but large.** At $n_\kappa \sim 20$, the constant factors in $O(N_b^2 n_\kappa^4)$ can dominate for small–medium systems. The crossover at 15–20 atoms means DG only wins beyond classically tractable regime (which is the point, but also means the advantage is hard to verify classically).

2. **Tested only on hydrogen chains.** The 1D geometry is favorable for block partitioning. 3D molecular geometries with multiple non-equivalent atoms, irregular coordination, or transition metal centres may require larger $n_\kappa$ or more sophisticated partitioning strategies.

3. **Pseudopotential assumption.** The plane-wave dual calculations use pseudopotentials to keep the kinetic energy cutoff manageable. All-electron calculations would require much larger primitive bases, potentially weakening the crossover.

4. **No rigorous error analysis.** The SVD truncation tolerance $\tau$ is a practical parameter; there's no theorem relating $\tau$ to the resulting energy error. The paper shows empirical insensitivity, but a formal bound would strengthen the method.

5. **The hybrid active space is ad hoc.** The weighting factor $\alpha = 0.01$ for combining UHF and Gaussian density matrices is empirically chosen. The paper acknowledges this and defers systematic optimization.

6. **No explicit compilation.** The paper discusses swap network structure and LCU cost models but doesn't compile the DG approach to concrete T-gate counts or surface-code resource estimates. Follow-up work is needed to compare against the explicit compilations in [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] or [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. (2021)]].

## Reusable ideas

1. [[DG Blocking for Block-Diagonal Hamiltonians]] — The SVD-based compression of delocalized active orbitals into block-local functions that preserve the diagonal property of the primitive basis. A general basis-engineering strategy.

2. [[Block-Diagonal Swap Network for Trotter Steps]] — Generalization of the [[Fermionic Swap Network|linear fermionic swap network]] to exploit block structure in the two-electron integrals, interpolating between $O(N)$-depth (diagonal) and $O(N^3)$-depth (fully general) regimes.

3. [[Hybrid Active Space Construction]] — Combining density matrices from complementary methods (UHF for static correlation, Gaussian basis for dynamic correlation) with weighted natural orbital truncation. Enables high-accuracy DG basis without expensive correlation calculations.

## References within this paper

- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe et al. (2018)]]: Introduced the [[Plane-Wave Dual Basis]] for quantum simulation of periodic systems. This paper builds on the dual basis as one of its primitive bases, extending its applicability to molecular (non-periodic) systems via DG blocking.
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al. (2018)]]: The [[Double Factorization of Two-Electron Integrals]] approach is used as an alternative Trotter decomposition strategy within DG blocks (Appendix A).
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean et al. (2018)]]: The [[Fermionic Swap Network]] is the foundation for the DG swap network construction.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]]: [[Qubitization (Quantum Walk for Spectral Encoding)|Qubitization]] and [[QROM (Quantum Read-Only Memory)|QROM]] provide the fault-tolerant cost model.
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven (2019)]]: First-quantized interaction picture for diagonal bases — the DG approach can reduce $N_p$ toward $N_d$ while retaining similar scaling.
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|Babbush et al. (2018)]]: First-quantized CI approach costs $O(N_a^3)$ in MO basis; DG basis could lower this to $O(N_d)$-type scaling in the integral-count dimension.
- Berry, Gidney, Motta, McClean, Babbush (2019, arXiv:1902.02134): Qubitization of arbitrary basis chemistry via low-rank factorization. The DG approach feeds into the same cost model with reduced $L$ and $\lambda$.
- O'Gorman, Huggins, Rieffel, Whaley (2019, arXiv:1905.05118): Generalized swap networks for non-diagonal Hamiltonians. The DG paper's swap network construction uses primitives from this work (double bipartite and balanced double bipartite swap networks).
- White (2017) and White & Stoudenmire (2019): Gausslet basis sets with diagonal Coulomb operator, used as primitive basis for the correlated DG calculations.
- Lin, Lu, Ying, E (2012); Hu, Lin, Yang (2015): Original DG framework for DFT calculations. This paper adapts the DG idea for correlated electronic structure without requiring surface correction terms.

## Cross-links

### Paper notes
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]]
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]]
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]]
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]]
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — the real-space grid precursor to the plane-wave dual primitive basis this paper builds on; Kivlichan et al. (2017) showed that position-space first quantisation gives $\tilde{O}(\eta^2)$ with a diagonal Hamiltonian — the same structural property that makes the DG primitive basis advantageous
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — extends qubitization to periodic Gaussian bases (Bloch orbitals); DG blocking could in principle compress Bloch integrals the same way it compresses MO integrals, reducing the $O(N_k^3 N^4)$ data by a block-locality factor
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — uses arithmetic-over-QROM rather than the diagonal primitive basis to handle pseudopotentials in first quantisation; complementary approach to the DG strategy of compressing from the primitive-basis side

### Trick cards
- [[DG Blocking for Block-Diagonal Hamiltonians]]
- [[Block-Diagonal Swap Network for Trotter Steps]]
- [[Hybrid Active Space Construction]]
- [[Fermionic Swap Network]]
- [[Plane-Wave Dual Basis]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Truncated Taylor Series Simulation]]
- [[Coherent Alias Sampling for PREPARE]]
