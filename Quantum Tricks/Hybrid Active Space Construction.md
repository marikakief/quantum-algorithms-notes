
> **Source:** McClean, Faulstich, Zhu, O'Gorman, Qiu, White, Babbush, Lin, arXiv:1909.00028
> **Tags:** #trick #basis-set #active-space #static-correlation #dynamic-correlation #DMRG #natural-orbitals

## What it does

Constructs a balanced active space that captures both static and dynamic electron correlation by combining density matrices from complementary methods — a large flexible basis (UHF in Gausslets) for static correlation and an empirically optimized Gaussian basis (cc-pVDZ) for dynamic correlation — then applying natural orbital truncation.

## The trick

Two density matrices are computed:
1. $D_{\text{UHF}}$: from an unrestricted Hartree-Fock calculation in a large primitive basis (e.g., Gausslets with $\sim 10{,}000$ functions). This captures static correlation and gives near-CBS HF accuracy. The UHF calculation is cheap despite the large basis because of sparsity in the diagonal primitive representation.
2. $D_{\text{Gaussian}}$: from the Gaussian cc-pVDZ basis, projected onto the same primitive basis. This basis has been empirically refined over decades to capture features important for dynamic correlation.

Form the combined density matrix:

$$D = D_{\text{UHF}} + \alpha\, D_{\text{Gaussian}}$$

with $\alpha \approx 0.01$ (small weighting ensures the Gaussian features augment rather than dominate). The leading natural orbitals of $D$ define the active space $\Phi$, which is then processed through [[DG Blocking for Block-Diagonal Hamiltonians|DG blocking]] to produce a block-diagonal basis.

The result: a basis of $\sim 150$ functions (for H$_{10}$) that preserves both the static correlation from the large Gausslet basis and the dynamic correlation features from the Gaussian basis, while maintaining block-diagonal two-electron integrals. DMRG on this basis gives energies near the complete basis set limit with 1–2 orders of magnitude less computational cost than either the pure Gausslet or pure Gaussian approach.

## When to reach for it

- You need correlated calculations (DMRG, CCSD, etc.) that capture both static and dynamic correlation, but using a single basis either gives too many functions (Gausslets) or misses important physics (small Gaussian basis).
- You want to build an active space for quantum simulation that's more accurate than a standard Gaussian active space without paying the full cost of a large primitive basis.
- Your system has significant multireference character (static correlation) that a standard Gaussian HF reference misses.

## Complexity

- Classical preprocessing: one UHF calculation in the large primitive basis (cheap due to diagonal interactions), one natural orbital truncation, one SVD for DG blocking.
- Resulting basis: typically $\sim 3\times$ the size of a cc-pVDZ basis (e.g., 150 vs 50 spatial orbitals for H$_{10}$), but with block-diagonal integrals and spatial locality.
- DMRG cost: $O(N_d D m^3)$ where $D$ is the MPO bond dimension (bounded by block locality) and $m$ is the state bond dimension. Comparable $D$ to the Gausslet basis, but $N_d \ll N_p$.

## Caveat

The weighting parameter $\alpha = 0.01$ is empirically chosen and not optimized. The paper explicitly defers systematic study of this parameter. For systems very different from hydrogen chains (e.g., transition metal complexes with dense d-orbital manifolds), the optimal weighting and the number of natural orbitals to retain may differ substantially. The approach also inherits the orthogonality concerns of combining non-identical basis sets, which the SVD in the DG procedure absorbs but doesn't formally resolve.

## Related notes
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]]
- [[DG Blocking for Block-Diagonal Hamiltonians]]
- [[Active Space Restriction for VQE (CAS-UCC)]]
- [[Orbital Relaxation via RDM Postprocessing]]
