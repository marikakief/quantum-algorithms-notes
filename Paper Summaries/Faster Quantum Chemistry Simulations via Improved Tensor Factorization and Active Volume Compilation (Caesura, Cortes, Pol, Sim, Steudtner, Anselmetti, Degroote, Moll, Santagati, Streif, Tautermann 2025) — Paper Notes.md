> **Source:** Athena Caesura, Cristian L. Cortes, William Pol, Sukin Sim, Mark Steudtner, Gian-Luca R. Anselmetti, Matthias Degroote, Nikolaj Moll, Raffaele Santagati, Michael Streif, Christofer S. Tautermann, *Faster quantum chemistry simulations on a quantum computer with improved tensor factorization and active volume compilation*, arXiv:2501.06165, Phys. Rev. Research (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2501.06165) · [Journal](https://doi.org/10.1103/yngp-5fpm)
> **Tags:** #quantum-chemistry #resource-estimation #fault-tolerant #THC #BLISS #active-volume #photonic #qubitization #FeMoCo #P450

---

## The computational problem

Electronic structure calculations of molecular systems via quantum phase estimation on fault-tolerant hardware. Primary target: cytochrome P450 (58 orbitals, 63 electrons) — an industrially and biologically relevant system for drug design. Secondary benchmark: FeMoCo (54e, 54o and the larger 113e, 76o active space).

Goal: reduce estimated runtimes to the point of practical relevance, using a photonic FTQC architecture based on fusion-based quantum hardware ("Active Volume" architecture).

---

## What the paper does

This paper delivers two independent advances stacked on top of existing qubitization + THC approaches:

1. **BLISS-THC factorization:** Combines Tensor Hypercontraction (THC) with the Block-Invariant Symmetry Shift (BLISS) method to achieve tighter Hamiltonian factorizations than either technique alone. BLISS shifts symmetry operators (particle number, spin projection) to reduce the 1-norm while preserving the spectrum of the Hamiltonian projected onto physical sectors. THC then compresses the shifted Hamiltonian more efficiently.

2. **Active Volume (AV) compilation:** Compiles the resulting quantum circuits for a fusion-based photonic FTQC platform using the AV architecture, which eliminates connectivity overheads of surface-code implementations and reduces runtime by fully utilizing the available hardware volume.

Combined, these two advances deliver a $\sim 100\times$ speedup in estimated runtime over prior art (THC, Lee et al. 2021) run on comparable hardware, primarily for P450. For FeMoCo, the improvements are cited in an appendix.

---

## The algorithm / construction

### BLISS-THC

**Step 1: BLISS preprocessing.** Add symmetry operators $\hat{S}$ to the Hamiltonian:

$$H \to H + \sum_k \mu_k \hat{S}_k$$

where $\hat{S}_k \in \{\hat{N}, \hat{N}^2, \hat{S}_z, \hat{S}_z^2, \ldots\}$ are symmetry operators that commute with $H$ (conserved quantities), and $\{\mu_k\}$ are free parameters. Since $\hat{S}_k$ annihilates energy eigenvalue differences (all eigenvalues in the physical sector shift by the same amount), the shifted $\tilde{H} = H + \sum_k \mu_k \hat{S}_k$ has the same eigenvalue spectrum on the physical subspace. However, its 1-norm $\lambda(\mu)$ depends on $\mu$ and can be minimized over $\mu$.

**Step 2: THC factorization of $\tilde{H}$.** Apply THC to decompose the two-electron integrals of the shifted Hamiltonian:

$$\tilde{h}_{pqrs} \approx \sum_{\mu\nu} Z_{\mu\nu} \chi_{\mu p} \chi_{\mu q} \chi_{\nu r} \chi_{\nu s}$$

where $\chi_{\mu p}$ are the THC leaf vectors (dimension $N \times M$ with $M \ll N^2$) and $Z_{\mu\nu}$ is a central tensor. The THC 1-norm is:

$$\lambda_{\rm THC} = \frac{1}{2}\sum_{\mu\nu} |Z_{\mu\nu}| \|\chi_\mu\|^2 \|\chi_\nu\|^2$$

**Combined optimization:** Optimize $\{Z_{\mu\nu}, \chi_{\mu p}, \mu_k\}$ jointly to minimize $\lambda_{\rm BLISS-THC}$. The BLISS degrees of freedom effectively provide a pre-conditioning step that makes the THC optimization landscape better-conditioned.

### Active Volume compilation

The AV architecture for photonic FTQC implements logical operations as "Active Volume blocks" — units of computation that execute on a fixed spatial footprint of physical qubits in each logical cycle. Key properties:
- Eliminates the "long-range routing" overhead of planar surface codes
- Parallelizes T-gate (Toffoli) factories efficiently
- Compilation: a greedy scheduler assigns each logical qubit a role per cycle

Runtime formula:

$$t_{\rm wall} = N_{\rm cycles} \cdot t_{\rm cycle}$$

where $N_{\rm cycles}$ is determined by the circuit depth after AV compilation and $t_{\rm cycle}$ is the photon-round-trip time. The improvement over surface code routing can be $10$–$50\times$ for dense circuits.

### Circuit construction

PREPARE and SELECT follow the standard qubitization template with THC:
- PREPARE: QROM load of $Z_{\mu\nu}$ values, coherent alias sampling
- SELECT: controlled Givens rotation networks (one per THC leaf)
- BLISS correction: adds a diagonal one-body correction term that is cheap to implement ($O(N)$ gates)

---

## Key results

**FeMoCo (113e, 76o — larger active space):**
- Toffoli count: $4.3 \times 10^9$
- Logical qubits: 1,512
- This active space includes more orbitals than the Reiher (54e/54o) benchmark, capturing more dynamical correlation

**FeMoCo (54e, 54o — Reiher active space), appendix:**
- Competitive with [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]] THC estimate; some improvement from BLISS

**Cytochrome P450 (58o, 63e) — primary result:**
- BLISS-THC achieves tighter factorization than pure THC
- AV compilation reduces runtime by $\sim 100\times$ over prior art at fixed hardware assumptions
- Practical runtime estimates (assuming photonic FTQC at specific device footprints) given as function of footprint

**BLISS-THC 1-norm improvement:**
- $\sim 25\%$ reduction in $\lambda$ vs. THC alone for P450 and FeMoCo — in line with subsequent [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low et al. 2025]] characterization

---

## Comparison with prior work

| Method | FeMoCo Toffolis | System | Key improvement |
|---|---|---|---|
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]] (THC) | $5.3 \times 10^9$ | 54e/54o | THC baseline |
| [[Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization (Oumarou, Scheurer, Parrish, Hohenstein, Gogolin 2024) — Paper Notes|Oumarou et al. 2024]] (RC-DF) | $2.6 \times 10^9$ | 54e/54o | Regularized DF |
| **Caesura et al. 2025 (BLISS-THC + AV)** | $\mathbf{4.3 \times 10^9}$ | **113e/76o** | **BLISS + AV compilation** |
| [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low et al. 2025]] (DFTHC+BLISS+SA) | $3.4 \times 10^8$ | 54e/54o | Spectrum amplification |

Note: the 4.3×10⁹ figure is for a larger (113e, 76o) active space — comparing to 54e/54o numbers directly undersells the improvement.

---

## Limits / caveats

- **Architecture-specific:** The $100\times$ speedup is relative to surface-code-compiled variants, not necessarily vs. any FTQC platform. Fusion-based photonic QC has different physical error rates and overhead.
- **AV compilation maturity:** The AV scheduler is a research prototype; actual deployable compilation toolchains may add overhead.
- **P450-focused:** The headline results are for P450; FeMoCo results are in the appendix and less detailed.
- **Active-space problem remains:** Like all qubitization papers, the approach targets a fixed active space. Dynamical correlation in P450 and FeMoCo requires convergence studies far beyond current estimates.
- **BLISS optimality:** The BLISS parameters are optimized numerically; the optimization landscape can have local minima. No guarantee of global optimality.

---

## Reusable ideas

1. [[Symmetry-Adapted Block Encoding via QROAM Data Reduction]] — BLISS shifts symmetry operators to reduce 1-norm without changing physical eigenvalues.

2. [[DFTHC Factorization]] — The BLISS-THC approach combines two ideas: pre-conditioning via symmetry and compression via THC. The joint optimization is the key.

3. **Active Volume Architecture** — A photonic FTQC compilation strategy that eliminates planar connectivity overhead by allowing arbitrary qubit-to-qubit connections in each cycle.

4. [[THC Non-Orthogonal Diagonal Coulomb Representation]] — Standard THC is the backbone of the factorization.

---

## Cross-links

### Paper notes
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC baseline that BLISS-THC improves on.
- [[Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization (Oumarou, Scheurer, Parrish, Hohenstein, Gogolin 2024) — Paper Notes]] — Competing approach from 2024; RC-DF vs BLISS-THC represent different strategies for compressing the same Hamiltonian.
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]] — Subsequent work that achieves $3.4 \times 10^8$ Toffolis for FeMoCo using DFTHC + BLISS + spectrum amplification, an additional $\sim 12\times$ improvement.
- [[Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) — Paper Notes]] — DF baseline.
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — P450 resource estimates from an earlier approach.
- [[FeMoCo Resource Estimation Timeline]] — Timeline of FeMoCo estimates.

### Trick cards
- [[DFTHC Factorization]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[Symmetry-Adapted Block Encoding via QROAM Data Reduction]]
- [[Qubitization Iterate]]
