> **Source:** Joshua J. Goings, Alec White, Joonho Lee, Christofer S. Tautermann, Matthias Degroote, Craig Gidney, Toru Shiozaki, Ryan Babbush, Nicholas C. Rubin, *Reliably assessing the electronic structure of cytochrome P450 on today's classical computers and tomorrow's quantum computers*, arXiv:2202.01244, PNAS **119**, e2203533119 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2202.01244) · [Journal](https://doi.org/10.1073/pnas.2203533119)
> **Tags:** #quantum-chemistry #quantum-advantage #resource-estimation #qubitization #tensor-hypercontraction #DMRG #surface-code #fault-tolerant #multireference #active-space #heme #P450

---

## The computational problem

Given a biologically relevant enzyme (cytochrome P450, specifically the catalytically active Compound I), determine the spin-state ordering of nearly degenerate doublet and quartet states to chemical accuracy. This requires balancing static and dynamic electron correlation in active spaces up to 58 orbitals / 63 electrons, a regime where no single classical method provides reliable results.

The paper treats this as a joint classical-quantum resource estimation problem: how much does it cost to solve this problem on today's classical hardware (DMRG+NEVPT2, CCSD(T)) versus tomorrow's fault-tolerant quantum computers ([[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] phase estimation)?

---

## What the paper does

Constructs a hierarchy of active spaces (5 to 58 orbitals) for four CYP model compounds and benchmarks classical methods against quantum resource estimates to define a quantum advantage boundary for a *pharmaceutically relevant* system. The key finding: for active spaces large enough to balance static and dynamic correlation (≥43 orbitals), [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] phase estimation with [[THC Non-Orthogonal Diagonal Coulomb Representation|THC]] factorization already shows more favorable runtime scaling than DMRG at high bond dimension. The largest system (58-orbital Cpd I) can be simulated with ~4.6 million physical qubits in 73 hours at 0.1% physical error rate, or ~500K qubits in 25 hours at 0.001% error rate.

My assessment: this paper is a model for how quantum advantage claims *should* be made — grounded in real chemistry rather than exotic molecules, with careful classical benchmarking to establish that the problem is actually hard. The honest conclusion that even quantum computers may struggle with full-system simulation (1.5 trillion Toffolis for 500 orbitals) is refreshing. The choice of CYP over FeMoCo is smart: CYPs metabolize >70% of marketed drugs, so the pharmaceutical relevance is immediate and concrete.

---

## The system: Cytochrome P450

CYP enzymes are membrane-bound heme-iron monooxygenases responsible for drug metabolism — CYP 3A4 and CYP 2D6 alone metabolize >70% of marketed drugs. The catalytic cycle passes through Compound I (Cpd I), a polyradical iron-oxo species whose spin-state ordering (doublet vs quartet) controls reactivity but remains unresolved.

Four model compounds are studied, derived from CYP3A4 crystal structures with surrounding protein removed:
1. **Resting state** — water bound to heme-iron (6-coordinate)
2. **Empty (pentacoordinate)** — no sixth ligand
3. **Pyridine inhibitor** — nitrogen heterocycle bound to iron (relevant to drug-drug interactions)
4. **Compound I (Cpd I)** — iron-oxo species, the catalytically active intermediate

---

## Active space hierarchy

The authors build nested active spaces by starting from 5 singly-occupied orbitals and systematically adding shells of chemically relevant orbitals, maintaining ~50% filling fraction:

| Active space | Added orbitals | Size (Cpd I) |
|---|---|---|
| A | Singly-occupied | 5 orbitals |
| B | Fe 3d, 4s, Fe-N antibonding | 8 |
| C | Fe-axial bonding, Fe 4p/d' | 15 |
| D | Fe-N σ bonding/antibonding | 23 |
| E | Heme π (near N) | 31 |
| F | Heme π (near C) | 41 |
| G | Cys C-S bonding/antibonding | 43 |
| X | Heme C-N σ bonding/antibonding | 58 |

This hierarchical construction — designed for convergence analysis rather than minimal cost — is itself a useful methodological contribution. Most prior studies optimize for the smallest active space that captures the physics; here the goal is to track how results evolve with systematic expansion.

---

## Classical results

### DMRG + CCSD(T) in active spaces

- **Sextet and quartet:** CCSD(T) and DMRG agree to within 0.1 kcal/mol across all active spaces → single-reference character
- **Doublet:** DMRG energy consistently much lower than CCSD(T) → multiconfiguational character (three open-shell natural orbitals: two Fe-O π* and one sulfur lone pair)
- **Spin gap at G (43 orbitals):** Doublet and quartet nearly degenerate (0.02 kcal/mol splitting). Sextet is 42.7 kcal/mol higher. This is the largest DMRG calculation performed on any Cpd I model at the time of publication.

### The dynamic correlation problem

Adding NEVPT2 corrections to account for dynamic correlation *outside* the active space can qualitatively change the spin ordering — in some active spaces, the sextet becomes lowest. Without larger active spaces where NEVPT2 corrections are smaller and more reliable, you can't distinguish real effects from approximation artifacts.

This is the central tension: the problem isn't "strongly correlated" in the traditional sense (no huge number of strongly entangled orbitals), but reliable simulation still demands multireference methods because balancing dynamic and static correlation across different spin states is inherently difficult.

### DMRG cost estimates for X active space

| Configuration | CPU time (hrs) | Memory (GB) | Disk (GB) |
|---|---|---|---|
| G, M=1500 (actual) | 1,800 | 48 | 235 |
| X, M=1500 (estimated) | 4,570 | 87 | 572 |
| X, M=3000 (estimated) | 36,564 | 348 | 2,288 |

DMRG scaling for fixed bond dimension: $O(k^3 M^3)$ CPU time, $O(k^2 M^2)$ memory. The X active space at M=3000 would require ~1 month wall time with aggressive parallelism, with no guarantee that M=3000 is sufficient.

---

## Multireference diagnostics

The paper carefully evaluates four diagnostics across all compounds and spin states:

1. **Spin contamination** $\langle S^2 - S_z^2 - S_z \rangle$: Large with HF orbitals for all compounds, but eliminated by switching to Kohn-Sham (DFT) orbitals for everything except the Cpd I doublet. This is [[Artificial vs Essential Spin Symmetry Breaking|"artificial" vs "essential" symmetry breaking]].

2. **max(|t₁|) from CCSD:** Large (>0.19) with HF orbitals; reduced with KS orbitals for all compounds except Cpd I. Since t₁ amplitudes are essentially orbital rotations, large values indicate poor reference orbitals rather than strong correlation.

3. **max(|t₂|) from CCSD:** ~0.05 for all compounds, roughly constant across reference choices. Since doubles capture genuine pairwise correlations (not orbital rotations), this is the more reliable diagnostic. None of the compounds show max(|t₂|) > 0.1.

4. **κ-OOMP2 NOONs:** Three NOONs near 1.0 for the Cpd I doublet (triradical character), consistent with DMRG. All other compounds show ideal open-shell counts.

The conclusion: Cpd I has multiconfiguational character (triradical doublet), but is not "strongly correlated" in the many-body sense. The difficulty is representing a triradical faithfully with a single determinant — you can't, and this matters for spin-gap energetics.

---

## Quantum resource estimates

### Three factorization schemes compared

The paper compares [[Single Factorization for Qubitized Chemistry|single factorization]] (SF), [[Double Factorization of Two-Electron Integrals|double factorization]] (DF), and [[THC Non-Orthogonal Diagonal Coulomb Representation|THC]] for [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] phase estimation:

| Method | Space complexity | Toffoli complexity | Empirical Toffoli scaling |
|---|---|---|---|
| SF | $\widetilde{O}(N^{3/2})$ | $\widetilde{O}(N^{3/2}\lambda/\varepsilon)$ | $O(N^{4.1})$ |
| DF | $\widetilde{O}(N\sqrt{\Xi})$ | $\widetilde{O}(N\sqrt{\Xi}\lambda/\varepsilon)$ | $O(N^{3.0})$ |
| THC | $\widetilde{O}(N)$ | $\widetilde{O}(N\lambda/\varepsilon)$ | $O(N^{2.5})$ |

THC wins decisively. The empirical $O(N^{2.5})$ Toffoli scaling for CYP models is consistent with the earlier FeMoCo results from [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. (2021)]].

### THC rank convergence (Cpd I, X active space)

THC rank $M = 220$ gives CCSD(T) error below 1 mHa with $\lambda = 385$, requiring $7.0 \times 10^9$ Toffolis and 1,429 logical qubits. The empirical slope of THC rank vs orbital count is 4.7, consistent with the ~5× rule from prior chemistry studies.

### Surface code compilation (largest systems, THC)

| Configuration | Physical qubits | Runtime | Error rate |
|---|---|---|---|
| 4 factories, 0.1% error | 4,624,440 | 73 hours | 0.1% |
| 4 factories, 0.001% error | ~500,000 | 25 hours | 0.001% |

AutoCCZ factories with level-1 code distance 19, level-2 code distance 31, data qubit code distance 29.

### The advantage boundary

Plotting DMRG CPU time against QPU time as a function of active space size: for bond dimension M ≥ 1000 and active spaces ≥ 31 orbitals, quantum phase estimation has a runtime advantage. The crossover happens earlier than for FeMoCo because the CYP active spaces are specifically designed to test convergence across a range of sizes.

Important caveat: QPE only gives energies. DMRG gives density matrices (and thus other observables). NEVPT2 corrections require 1-, 2-, 3-, and sometimes 4-particle RDMs, which QPE doesn't directly provide. The paper flags this as a gap requiring further quantum algorithm development.

### Full-system extrapolation

Extrapolating to 500 orbitals (the full valence space used in classical coupled cluster calculations): ~9,000 logical qubits and 1.5 trillion Toffolis. This is currently infeasible for any foreseeable quantum device, suggesting that quantum algorithms addressing dynamic correlation (rather than brute-force simulation of the full space) are needed.

---

## Initial state overlap

The largest computational basis state overlap with the DMRG M=1500 ground state decays from ~1.0 (5 qubits) to ~0.6 (84 qubits) across active spaces A through G. This is slow enough that simple Hartree-Fock initial states should suffice for QPE — no sophisticated state preparation is needed, at least for these active space sizes. This is consistent with the observation from [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman et al. (2018)]] that the orthogonality catastrophe is less severe than sometimes feared for moderate active spaces.

---

## Comparison with prior work

| System | Paper | Logical qubits | Toffoli count | Runtime |
|---|---|---|---|---|
| FeMoCo (Reiher, 108 SO) | [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes\|Lee et al. 2021]] | 2,142 | $5.3 \times 10^9$ | ~4 days |
| CYP Cpd I (X, 58 orb) | This paper | 1,429 | $7.0 \times 10^9$ | 73 hours |
| CYP full space (~500 orb) | This paper (extrapolated) | ~9,000 | $1.5 \times 10^{12}$ | Infeasible |

The CYP active space is smaller than FeMoCo's, but the per-orbital cost is comparable. The key difference is the paper's dual approach: classical benchmarks *and* quantum estimates on the same system, enabling direct comparison.

---

## Limits / caveats

1. **QPE only gives energies.** Dynamic correlation corrections (NEVPT2) require RDMs that QPE doesn't provide. This limits the practical utility of QPE for the "balanced correlation" problem the paper identifies.

2. **Active space models are not the full protein.** The model compounds strip away the protein environment, dielectric effects, and full cysteine ligand. These can shift spin energetics by enough to change qualitative conclusions.

3. **Resource estimates are upper bounds.** The surface code compilation assumes specific factory counts and timing parameters. Improvements in hardware or error correction could significantly reduce costs.

4. **The "advantage" is against DMRG, not all classical methods.** AFQMC, selected CI, and other classical methods may have different scaling profiles. The paper compares against DMRG because it's the most systematically improvable multireference method, but the advantage boundary could shift.

5. **THC decomposition quality matters.** The paper introduces an improved L1-regularized THC optimization (CP3 initial guess + L-BFGS-B), which is necessary to avoid erratic $\lambda$ values. The THC rank-to-orbital ratio of 4.7 is system-specific; other molecules may require higher ratios.

6. **Full-system simulation is infeasible on either platform.** 500-orbital QPE requires 1.5T Toffolis. This is a sobering reminder that quantum advantage for chemistry may require algorithmic advances beyond brute-force active space simulation.

---

## Reusable ideas

1. [[Hierarchical Active Space Construction for Convergence Analysis]] — Nested active space hierarchy designed for systematic convergence analysis rather than minimal cost; each level adds chemically motivated orbital shells at ~50% filling fraction.

2. [[Classical-Quantum Runtime Crossover Analysis]] — Direct comparison of DMRG CPU time (at varying bond dimension) against QPE runtime (at varying factory count) on the same Hamiltonian, enabling an empirical advantage boundary.

3. [[Artificial vs Essential Spin Symmetry Breaking]] — Distinguishing "artificial" symmetry breaking (corrected by switching to KS reference orbitals) from "essential" symmetry breaking (persists regardless of reference) as a diagnostic for multiconfiguational character.

4. [[L1-Regularized THC Optimization from CP3]] — Using symmetric canonical polyadic decomposition (CP3) of Cholesky vectors as initial guess for THC, followed by L1-regularized L-BFGS-B optimization to simultaneously minimize factorization error and the LCU 1-norm $\lambda$.

---

## References within this paper

- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. (2021)]] — The THC qubitization framework used for all quantum resource estimates; provides the walk operator construction and cost formulas
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney et al. (2019)]] — Single factorization qubitization; one of the three methods compared
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — Original qubitization framework
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta et al. (2018)]] — Double factorization of two-electron integrals; one of the three methods compared
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman et al. (2018)]] — Initial state overlap considerations for QPE
- Berry, Kieferová, Scherer, Sanders, Low, Wiebe, Gidney, Babbush (2018) — Improved QPE cost scaling; $O(\lambda/\varepsilon)$ query complexity for qubitized walks
- Gidney and Fowler (2019) — Surface code AutoCCZ magic state factory design used in physical resource estimates
- von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer (2021) — Double factorization qubitization; compared in the three-method benchmarks
- Chen, Lai, Shaik (2011) — Prior CASSCF/CASPT2 calculations on Cpd I establishing triradical character
- Olivares-Amaya et al. (2015) — DMRG energy extrapolation methodology based on discarded weight

---

## Cross-links

- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — pioneered the quantum-as-coprocessor workflow for enzyme catalysis that this paper applies to CYP P450

### Paper notes
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC qubitization framework; this paper applies it to CYP models
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — Single factorization qubitization compared here
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Original qubitization paper
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — Double factorization compared here
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — Related tensor factorization work
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — Complementary analysis of the quantum advantage question; this paper provides a concrete case study supporting the "polynomial advantage possible, exponential advantage unlikely" conclusion
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — Initial state overlap; CYP results confirm moderate overlap decay
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — Alternative (first-quantized) resource estimation approach
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — QC-QMC as an alternative hybrid approach that could address the dynamic correlation gap this paper identifies
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al 2022]]
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al 2023]]

### Trick cards
- [[THC Non-Orthogonal Diagonal Coulomb Representation]] — The THC technique that gives lowest resource estimates here
- [[Double Factorization of Two-Electron Integrals]] — One of the three factorization methods compared
- [[Single Factorization for Qubitized Chemistry]] — One of the three factorization methods compared
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — The quantum walk framework underlying all resource estimates
- [[Coherent Alias Sampling for PREPARE]] — Used in PREPARE oracle construction
- [[QROM (Quantum Read-Only Memory)]] — Used for loading THC/rotation data
- [[QROAM (Space-Time Tradeoff for QROM)]] — Space-time tradeoff for QROM in the walk operator
- [[EQA Assessment Framework (State Preparation vs Classical Heuristic)]] — This paper provides a concrete data point: CYP shows polynomial (not exponential) quantum advantage
- [[Hierarchical Active Space Construction for Convergence Analysis]] — New trick card from this paper
- [[Classical-Quantum Runtime Crossover Analysis]] — New trick card from this paper
- [[Artificial vs Essential Spin Symmetry Breaking]] — New trick card from this paper
- [[L1-Regularized THC Optimization from CP3]] — New trick card from this paper

See [[FeMoCo Resource Estimation Timeline]] for how this estimate fits in the broader progression.
