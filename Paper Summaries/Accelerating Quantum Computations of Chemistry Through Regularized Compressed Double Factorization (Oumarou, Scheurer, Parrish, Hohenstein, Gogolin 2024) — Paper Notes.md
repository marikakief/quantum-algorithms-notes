> **Source:** Oumarou Oumarou, Maximilian Scheurer, Robert M. Parrish, Edward G. Hohenstein, Christian Gogolin, *Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization*, arXiv:2212.07957, Quantum **8**, 1371 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2212.07957) · [Journal](https://doi.org/10.22331/q-2024-06-13-1371)
> **Tags:** #quantum-chemistry #resource-estimation #fault-tolerant #double-factorization #qubitization #FeMoCo #NISQ #1-norm-reduction

---

## The computational problem

Given the molecular electronic Hamiltonian expressed in second quantization, compute a compressed representation — the **regularized compressed double factorization (RC-DF)** — that simultaneously:
1. Minimizes the 1-norm $\lambda$ of the LCU decomposition (which controls QPE iteration count in qubitization-based algorithms)
2. Produces a representation suitable for both NISQ (reduced measurement count) and fault-tolerant (reduced Toffoli count) algorithms

Primary targets: cytochrome P450 (58 orbitals, 63 electrons) and FeMoCo active space models.

---

## What the paper does

RC-DF introduces regularization into the compressed double factorization (CDF) optimization problem. The standard CDF approach decomposes the Hamiltonian by choosing a set of rank-$R$ rotation matrices $U^{(\ell)}$ that minimize the 1-norm of the resulting factorization. RC-DF adds a CCSD(T)-motivated regularization term that ensures the factorized Hamiltonian approximates the energetics faithfully even when the factorization rank is low.

Key claims:
- For NISQ: RC-DF reduces measurement bases by $\sim 3\times$ and shot count to chemical accuracy by $3$–$6\times$ compared to truncated DF, outperforming Pauli grouping by an order of magnitude on 12–20 qubit systems
- For fault-tolerant: RC-DF cuts QPE runtime almost in half compared to truncated DF and outperforms THC on $\lambda$ for cytochrome P450 CpdI (58 orbitals)
- The approach bridges NISQ and fault-tolerant regimes with a single classical preprocessing step

The 2.6×10⁹ Toffoli figure for FeMoCo arises from applying RC-DF to the Reiher active space model and computing the resulting 1-norm $\lambda$ combined with standard qubitization resource estimates.

---

## The algorithm / construction

### Regularized compressed double factorization

Starting from the two-electron integrals $h_{pqrs}$ in chemist notation, CDF finds a set of rotation matrices $\{U^{(\ell)}\}_{\ell=1}^L$ that decompose:

$$H_{\rm 2e} \approx \frac{1}{2}\sum_\ell \left(\sum_j \lambda_j^{(\ell)} n_j^{(\ell)}\right)^2$$

where $n_j^{(\ell)} = \sum_\sigma (U^{(\ell)})^\dagger_{jq} a^\dagger_{q\sigma} a_{q\sigma}$ and the $\lambda_j^{(\ell)}$ are chosen to minimize a cost.

Standard CDF minimizes the 1-norm $\lambda = \sum_\ell \|\lambda^{(\ell)}\|_1^2 / 2$ directly. RC-DF instead optimizes:

$$\mathcal{L}(U^{(\ell)}, \lambda^{(\ell)}) = \lambda + \mu \cdot \delta E_{\rm CCSD(T)}$$

where $\delta E_{\rm CCSD(T)}$ measures how much the factorized Hamiltonian shifts CCSD(T) energies relative to the exact integrals, and $\mu$ is a regularization parameter. This prevents the optimizer from collapsing $\lambda$ at the cost of large energy errors.

The optimization is classical and uses a variant of alternating minimization over $U^{(\ell)}$ (Riemannian gradient descent on the Stiefel manifold) and $\lambda^{(\ell)}$ (unconstrained quadratic).

### Connection to NISQ measurements

For NISQ, each leaf $\ell$ defines a commuting set of fermionic observables in the rotated frame — a "fermionic fragment" — that can be measured with a single basis rotation circuit. The measurement count scales as the variance of each fragment:

$$M \sim \frac{\sum_\ell \text{Var}_{|\psi\rangle}[T_\ell^{(\ell)}]}{\varepsilon^2}$$

RC-DF's regularization term correlates with low variance, so minimizing the joint objective simultaneously reduces fault-tolerant $\lambda$ and NISQ measurement count.

### 1-norm and Toffoli counts

For qubitization, the total Toffoli count scales as:

$$T_{\rm total} \approx \frac{\pi \lambda}{2 \varepsilon} \cdot T_{\rm oracle}$$

where $\lambda$ is the 1-norm of the factorized Hamiltonian and $T_{\rm oracle}$ is the per-step Toffoli cost. RC-DF achieves lower $\lambda$ than both truncated DF and THC for the P450 benchmark, and competitive with THC for FeMoCo.

---

## Key results

**FeMoCo (Reiher active space, 54e/54o):**
- RC-DF 1-norm: $\lambda_{\rm RC-DF} \approx 700$ Ha (competitive with THC)
- Estimated Toffoli count: $\sim 2.6 \times 10^9$ (at target precision 1 mHa)
- Logical qubits: ~1,994
- Roughly $2\times$ improvement over THC baseline from [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]]

**Cytochrome P450 CpdI (58 orbitals):**
- RC-DF achieves $\lambda < \lambda_{\rm THC}$ — first to beat THC on $\lambda$ for P450
- NISQ shot count reduction: $3$–$6\times$ over truncated DF; $\sim 10$–$20\times$ over Pauli grouping

**CCSD(T) energy error reduction:**
- Regularization reduces energy error at fixed rank by $\sim 1$–$2$ orders of magnitude vs. unregularized CDF

---

## Comparison with prior work

| Method | FeMoCo Toffolis | P450 $\lambda$ advantage |
|---|---|---|
| [[Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) — Paper Notes|von Burg et al. 2020]] (DF) | $1.0 \times 10^{10}$ | Baseline |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]] (THC) | $5.3 \times 10^9$ | Beats DF by $\sim 1.5\times$ |
| **Oumarou et al. 2024 (RC-DF)** | $\mathbf{2.6 \times 10^9}$ | **Beats THC for P450** |
| [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low et al. 2025]] (DFTHC+BLISS+SA) | $3.4 \times 10^8$ | $\sim 16\times$ vs RC-DF |

RC-DF establishes that combining regularization with compression can beat pure 1-norm minimization (THC) by introducing chemical structure as an inductive bias.

---

## Limits / caveats

- **Classical cost:** The alternating minimization over rotation matrices is expensive. For large active spaces (100+ orbitals), the classical preprocessing can become a bottleneck.
- **P450 advantage doesn't always generalize:** RC-DF beats THC on P450 but the advantage depends on system structure. For FeMoCo, the improvement over THC is modest.
- **Regularization parameter tuning:** The $\mu$ parameter requires tuning per system; no universal recipe is provided.
- **Active-space limitation:** Like all qubitization approaches in this era, the estimates target a fixed active space. Dynamical correlation remains unaddressed.
- **NISQ relevance:** The NISQ measurement reduction is impressive for small systems, but the molecule sizes at which NISQ is relevant (12–20 qubits) are already classically tractable.

---

## Reusable ideas

1. [[Double Factorization of Two-Electron Integrals]] — Core technique; RC-DF is CDF with regularization.

2. [[Double Factorisation for Block-Encoding]] — Block encoding using the double-factorized Hamiltonian; RC-DF reduces the key 1-norm parameter.

3. [[Basis Rotation Grouping for VQE Measurement]] — In the NISQ context, each CDF leaf defines a measurable fermionic fragment; RC-DF minimizes total measurement cost.

4. **Regularized Compression as Inductive Bias** — Adding a physically motivated energy error penalty to a 1-norm minimization problem improves both accuracy and compactness simultaneously. Generalizes to other factorization approaches.

---

## Cross-links

### Paper notes
- [[Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) — Paper Notes]] — DF baseline; RC-DF improves on this by roughly $4\times$ for FeMoCo.
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC baseline; RC-DF beats THC on P450 $\lambda$ and approaches it for FeMoCo.
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — Original double factorization as a classical scheme.
- [[FeMoCo Resource Estimation Timeline]] — This paper is the 2024 entry in the FeMoCo resource estimate progression.
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]] — Subsequent work that combines DFTHC + BLISS + spectrum amplification to achieve $3.4 \times 10^8$ Toffolis.

### Trick cards
- [[Double Factorization of Two-Electron Integrals]]
- [[Double Factorisation for Block-Encoding]]
- [[Basis Rotation Grouping for VQE Measurement]]
- [[Qubitization Iterate]]
