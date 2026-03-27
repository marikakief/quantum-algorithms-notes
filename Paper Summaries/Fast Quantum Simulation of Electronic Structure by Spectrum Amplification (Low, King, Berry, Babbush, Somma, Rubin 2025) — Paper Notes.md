> **Source:** Guang Hao Low, Robbie King, Dominic W. Berry, Qiushi Han, A. Eugene DePrince III, Alec White, Ryan Babbush, Rolando D. Somma, Nicholas C. Rubin, *Fast Quantum Simulation of Electronic Structure by Spectrum Amplification*, arXiv:2502.15882, Phys. Rev. X **15**, 041016 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2502.15882) · [Journal](https://doi.org/10.1103/pb2g-j9cw)
> **Tags:** #quantum-chemistry #spectrum-amplification #sum-of-squares #block-encoding #qubitization #FeMoCo #fault-tolerant #tensor-factorization #DFTHC #resource-estimation #phase-estimation

---
## The computational problem

Estimate the ground-state energy $E_{\text{gs}}$ of a molecular electronic Hamiltonian

$$H = \sum_{pq} h^{(1)}_{pq} E_{pq} + \frac{1}{2}\sum_{pqrs} h^{(2)}_{pqrs} E_{pq} E_{rs}$$

(spin-free second-quantized form, $E_{pq} = \sum_\sigma a^\dagger_{p\sigma} a_{q\sigma}$) to chemical accuracy $\varepsilon_{\text{chem}} = 1.6\;\text{mHa}$ on a fault-tolerant quantum computer. The standard approach: compress the Coulomb operator via tensor factorizations, [[Qubitization (Quantum Walk for Spectral Encoding)|qubitize]] the resulting [[Linear Combination of Unitaries (LCU)|LCU]], and run [[Phase Estimation as Eigenvalue Filter for Walk-Based Search|phase estimation]]. The bottleneck is the [[Linear Combination of Unitaries (LCU)|LCU]] 1-norm $\Lambda$, which enters the total cost linearly: $O(\Lambda / \varepsilon)$.

This paper asks: can we break through the $O(\Lambda)$ barrier? Yes.

---
## What the paper does

By expressing $H$ as a sum of squares (SOS), the Hamiltonian becomes positive semidefinite (up to a shift $E_{\text{SOS}}$). Spectrum amplification then exploits the nonlinearity of $\sqrt{x}$ near zero: small eigenvalues $E_{\text{gap}} = E_{\text{gs}} - E_{\text{SOS}}$ of $H - E_{\text{SOS}}$ map to $\sqrt{E_{\text{gap}}}$ in the "square root" Hamiltonian $H_{\text{sqrt}}$. Phase estimation on $H_{\text{sqrt}}$ resolves $E_{\text{gs}}$ with effective normalization $\lambda_{\text{eff}} = \sqrt{2\Lambda E_{\text{gap}}}$ instead of $\Lambda$. Since $E_{\text{gap}} \ll \Lambda$ for real molecules, this is a major win.

To make this practical, they:

1. Introduce efficient quantum circuits for [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|spectrum amplification]] via rectangular [[Block-Encoding Composition Algebra|block-encodings]]
2. Show that SOS representations of chemistry Hamiltonians can be computed classically via semidefinite programming
3. Develop **DFTHC** (Double-Factorized Tensor Hypercontraction) — a new integral factorization that interpolates between [[Double Factorisation for Block-Encoding|double factorization]] and [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|tensor hypercontraction]]

Combined result: **4× to 195× speedup** over prior art for ground-state energy estimation of Fe-S clusters, FeMoCo, CO₂ catalysts, and cytochrome P450. For FeMoCo-76: $9.99 \times 10^8$ Toffolis, down from $4.3 \times 10^9$ (prior best, THC+BLISS).

---

## The algorithm / construction

### Step 1: SOS representation

Any Hamiltonian can be written as

$$H = \sum_\alpha O_\alpha^\dagger O_\alpha + E_{\text{SOS}} = H_{\text{sqrt}}^\dagger H_{\text{sqrt}} + E_{\text{SOS}}$$

where $H_{\text{sqrt}} = \sum_\alpha |\chi_\alpha\rangle \otimes O_\alpha$ is a rectangular operator and $E_{\text{SOS}}$ is chosen so that $H - E_{\text{SOS}} \succeq 0$. The key parameter is

$$\Delta_{\text{gap}} = \frac{E_{\text{gs}} - E_{\text{SOS}}}{\lambda_{\text{sqrt}}^2}$$

where $\lambda_{\text{sqrt}} = \sqrt{\sum_\alpha \lambda_\alpha^2}$ is the [[Block-Encoding Composition Algebra|block-encoding]] normalization. If $\Delta_{\text{gap}} \ll 1$, spectrum amplification gives a large advantage.

### Step 2: Finding the SOS decomposition classically

The paper solves the semidefinite program

$$\max\; E_{\text{SOS}} \quad \text{s.t.}\quad H - E_{\text{SOS}} I = \vec{o}^\dagger G \vec{o},\quad G \succeq 0$$

where $\vec{o}$ is a vector of fermionic operators. For the spin-free level-2 SOS algebra, the generators are:

$$O_\alpha = \sum_{j,\sigma} c^\alpha_{j\sigma} a_{j\sigma} + \sum_{j,\sigma} d^\alpha_{j\sigma} a^\dagger_{j\sigma} + \left(e^\alpha I + \sum_{ij} g^\alpha_{ij} \sum_\sigma a^\dagger_{i\sigma} a_{j\sigma}\right)$$

This is the simplest algebra that still gives useful gaps. Larger algebras (DQG, level-3) give tighter bounds but cost more to block-encode. The SDP is solved using the GPU-accelerated cuLoRADS solver with low-rank factorization.

Empirically, $E_{\text{gap}} \sim N^{0.88}$ and is on the order of a few Hartrees for systems with 20–150 orbitals. For FeMoCo-76: $E_{\text{gap}} \approx 5.38\;\text{Ha}$, $\Lambda \approx 179.7\;\text{Ha}$, $\lambda_{\text{eff}} \approx 43.7\;\text{Ha}$.

### Step 3: Rectangular block-encoding of $H_{\text{sqrt}}$

Previous approaches ([[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma-Boixo]]; Zlokapa & Somma, arXiv:2404.03644) used Hermitian dilations of $H_{\text{sqrt}}$, which need more qubits and are at least $2\times$ more expensive. This paper directly block-encodes the rectangular $H_{\text{sqrt}}$:

$$U = \text{Sel} \cdot \text{Prep}^\dagger = \text{Be}\!\left[\frac{H_{\text{sqrt}}}{\lambda_{\text{sqrt}}}\right]$$

with PREPARE loading $|\chi_\alpha\rangle$ states via [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] and SELECT applying controlled block-encodings of each $O_\alpha$.

A quantum walk $W_1 = \text{Ref}_B U$, $W_2 = \text{Ref}_{aB} U^\dagger$ on this rectangular [[Block-Encoding Composition Algebra|block-encoding]] has eigenphases $\arccos(\sqrt{E_j}/\lambda_{\text{sqrt}})$. The trick: two walk steps give $W_2 W_1$ with eigenphases $\arccos(2E_j/\lambda_{\text{sqrt}}^2 - 1)$, equivalent to a [[Block-Encoding Composition Algebra|block-encoding]] of $2H_{\text{SA}}/\lambda_{\text{sqrt}}^2 - I$. This restores compatibility with standard [[Phase Estimation as Eigenvalue Filter for Walk-Based Search|phase estimation]] while maintaining the spectrum amplification advantage.

### Step 4: DFTHC factorization

The DFTHC representation of the two-body tensor:

$$H_{\text{DFTHC}} = \sum_{pq} h'^{(1)}_{pq} E_{pq} + \frac{1}{2}\sum_{r \in [R]} \sum_{c \in [C]} \left(W^{(rc)} I + \sum_{b=0}^{B-1} w^{(rc)}_b \sum_{pq} u^{(r)}_{b,p} u^{(r)}_{b,q} E_{pq}\right)^2 - \frac{1}{2}\sum_{rc} (W^{(rc)})^2$$

Parameters $(R, B, C)$ interpolate between:
- **Double factorization:** $B = N$, $C = 1$ (many rotations, few coefficients)
- **THC:** $R = 1$, $B = \Theta(N)$, $C = \Theta(N)$ (few rotations, many coefficients)
- **DFTHC sweet spot:** $R = \tilde{O}(1)$, $B = \Theta(N)$, $C = \Theta(N)$ — balances the two dominant table-lookup oracles (Rot and Qroam)

This representation is already in SOS form: each squared term is a non-negative operator. The one-body terms split into D1 (positive eigenvalues) and Q1 (negative eigenvalues, rewritten via anticommutation) generators plus the spin-free SF generators.
### Step 5: Combined cost
The block-encoding normalization:

$$\Lambda = \frac{1}{2}\left(\lambda_{D1} + \lambda_{Q1} + \lambda_{\text{SF}}\right)$$

The effective LCU norm for phase estimation with spectrum amplification:

$$\lambda_{\text{eff}} = \sqrt{2\Lambda E_{\text{gap}}} \ll \Lambda$$

Total Toffoli count for chemical accuracy ($\sigma_{\text{PEA}} = 1\;\text{mHa}$):

$$\text{Toffolis} = \left\lceil\frac{\pi \lambda_{\text{eff}}}{2\sigma_{\text{PEA}}}\right\rceil \cdot \text{Cost}[\text{Be}]$$

The block-encoding cost per walk step is dominated by:
- **Rot** (basis rotations via [[QROM (Quantum Read-Only Memory)|QROM]]-loaded Givens angles): $\sim N + RB$ Toffolis
- **Qroam** (coefficient table lookup): $\sim RBC/(\lambda+1) + \lambda b_Q$ Toffolis
- **Sel** (Majorana operator application): $\sim 4(N-1)b_{\text{rot}}$ Toffolis

DFTHC balances Rot and Qroam costs, with Sel typically dominating at ~60% of total.

### Step 6: Optimization

DFTHC parameters are found by solving a nonlinear program via first-order gradient descent (Adam optimizer, auto-differentiated in JAX):

$$\min_{u, w, h^{(2)}_S} \left[\frac{\|h^{(2)}_S - h^{(2)'}\|_{\text{fro}}}{\varepsilon_{\text{reg}}} + \frac{\Lambda}{\lambda_{\text{reg}}} + \text{ReLU}\!\left(\frac{E_{\text{gap}} - E_{\text{reg}}}{E_{\text{reg}}}\right)\right]$$

This jointly minimizes the Frobenius error, block-encoding normalization, and SOS gap. The optimization uses BLISS (block-invariant symmetry shift) to further reduce $\Lambda$ within the $\eta$-particle sector.

---

## Key results

**FeMoCo resource estimates (chemical accuracy, $\sigma_{\text{PEA}} = 1\;\text{mHa}$):**

| System | Qubits | Toffolis | Improvement over DF | Improvement over THC |
|---|---|---|---|---|
| Fe₂S₂ [30e, 20o] | 466 | $3.97 \times 10^7$ | 11.7× | 11.7× |
| Fe₄S₄ [54e, 36o] | 873 | $1.72 \times 10^8$ | 23.3× | 13.2× |
| FeMoCo [54e, 54o] | 1,137 | $3.41 \times 10^8$ | 7.0× | 15.5× |
| FeMoCo [113e, 76o] | 1,459 | $9.99 \times 10^8$ | 37.1× | 4.3× |
| CPD1-P450X [63e, 58o] | 1,150 | $4.91 \times 10^8$ | 7.7× | 3.5× |
| CO₂ [64e, 56o] | 924 | $2.05 \times 10^8$ | 121.9× | — |
| CO₂ [100e, 100o] | 1,960 | $1.06 \times 10^9$ | 112.7× | — |
| CO₂ [150e, 150o] | 2,870 | $2.81 \times 10^9$ | 195.2× | — |

**Scaling (empirical fits across all benchmarks):**
- Toffolis per walk step: $\sim 0.079 \times N^{2.09}$ million
- $\Lambda \sim 0.23 \times N^{1.46}\;\text{Ha}$
- $\lambda_{\text{eff}} \sim 0.24 \times N^{1.14}\;\text{Ha}$
- $E_{\text{gap}} \sim 0.10 \times N^{0.88}\;\text{Ha}$

**Physical resource estimate (FeMoCo-76):** With 4 CCZ factories, $10^{-3}$ physical error rate, 1 µs surface code cycle, 10 µs reaction time: ~$2 \times 10^6$ physical qubits, ~4 hours runtime. Phase estimation cost ($9.99 \times 10^8$ Toffolis) is now comparable to state preparation cost ($7.6 \times 10^8$ to $1.3 \times 10^9$ Toffolis for MPS with 0.95–0.99 overlap).

---

## Comparison with prior work

| Method | Year | $\lambda$ | Block-encoding Toffolis | Total Toffolis (FeMoCo-76) |
|---|---|---|---|---|
| Trotterization | 2017 | — | — | $5.0 \times 10^{13}$ |
| Single factorization | 2019 | 584.5 Ha | 40,442 | $1.2 \times 10^{11}$ |
| [[Double Factorisation for Block-Encoding\|Double factorization]] | 2020 | 78.0 / 584.5 Ha | 19,589 / 40,442 | $2.3 \times 10^{10}$ / $5.3 \times 10^{10}$ |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes\|THC]] | 2020 | 306.3 / 198.9 Ha | 11,016 / 13,763 | $5.3 \times 10^9$ / $3.2 \times 10^{10}$ |
| Symmetry compression (DF) | 2024 | — | — | $2.6 \times 10^9$ |
| Symmetry compression (THC+BLISS) | 2025 | 198.9 Ha | 13,763 | $4.3 \times 10^9$ |
| **DFTHC+BLISS+SA (this paper)** | **2025** | **$\lambda_{\text{eff}} = 43.7$ Ha** | **14,563** | $\mathbf{9.99 \times 10^8}$ |

The improvement comes primarily from reducing $\lambda$: spectrum amplification converts $\Lambda \approx 180\;\text{Ha}$ to $\lambda_{\text{eff}} \approx 44\;\text{Ha}$, a $4\times$ reduction. The block-encoding cost per step is similar to THC, so the win is almost entirely in fewer phase estimation repetitions.

---

## Limits / caveats

1. **Classical preprocessing cost.** The SDP for the SOS decomposition scales polynomially but is expensive for large systems. The cuLoRADS GPU solver handles up to ~150 orbitals; scaling beyond this needs algorithmic improvements or approximations.

2. **SOS algebra choice.** The spin-free level-2 algebra is the simplest in the hierarchy. Tighter gaps from larger algebras (DQG, level-3) are possible but increase block-encoding cost. The trade-off between gap tightness and block-encoding cost is not fully understood.

3. **DFTHC optimization is non-convex.** The nonlinear program (Eq. 41) has many local minima. Results depend on initial conditions and hyperparameters. The paper addresses this with randomization (multiple restarts), but the search over $(R, B, C)$ is not exhaustive.

4. **Error budget sensitivity.** Chemical accuracy requires careful partitioning of error between $\sigma_{\text{PEA}}$ and $\varepsilon_{\text{corr}}$ (the DFTHC approximation error). The paper proposes a randomized truncation protocol where $\varepsilon_{\text{corr}}$ becomes zero-mean with controllable variance, allowing errors to add in quadrature rather than linearly. This is a genuinely nice trick — but it makes the results not directly comparable to prior work using worst-case error budgets.

5. **High-spin CCSD(T) as a quality metric is fragile.** Table VI shows that the CCSD(T) correlation energy difference between DFTHC and exact integrals fluctuates wildly and doesn't decrease monotonically with rank. For strongly correlated systems like Fe₄S₄, the DFTHC representation may describe fundamentally different physics at low truncation thresholds.

6. **$\lambda_{\text{eff}}$ within $2\times$ of lower bounds.** This constrains further improvement from the spin-free level-2 algebra — future gains need either a tighter SOS hierarchy or a fundamentally different approach.

7. **State preparation is now the bottleneck.** With phase estimation costing $\sim 10^9$ Toffolis, MPS state preparation (also $\sim 10^9$ Toffolis for 0.95 overlap on FeMoCo-76) is now comparable. Further reducing QPE cost has diminishing returns unless state preparation improves in parallel.

---

## Reusable ideas

1. [[Rectangular Block-Encoding for Spectrum Amplification]] — Block-encode a rectangular $H_{\text{sqrt}}$ directly instead of Hermitianizing; quantum walk on left/right singular spaces via alternating reflections; recovers standard phase estimation with two walk steps.

2. [[SOS Decomposition via Semidefinite Programming for Chemistry]] — Express chemistry Hamiltonians in sum-of-squares form via SDP dual of the reduced density matrix lower bound; spin-free level-2 algebra gives useful gaps at polynomial classical cost.

3. [[DFTHC Factorization]] — Interpolating ansatz between double factorization and tensor hypercontraction; three parameters $(R, B, C)$ trade off rotation oracle cost vs coefficient oracle cost; naturally SOS-compatible.

4. [[Randomized Error Budget for Phase Estimation]] — Randomize DFTHC initial conditions and bit truncation to make approximation error zero-mean; errors add in quadrature instead of linearly; allows larger $\sigma_{\text{PEA}}$ within the same total error budget.

---

## References within this paper

Key citations (vault notes linked where they exist):

- **[1]** Kitaev, *Quantum measurements and the abelian stabilizer problem* (1995) — original phase estimation
- **[7]** [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry, Babbush et al. (2021)]] — tensor hypercontraction for qubitization; prior best FeMoCo resource estimates
- **[9]** von Burg et al. (2020) — double factorization of Coulomb integrals; prior best DF resource estimates
- **[14]** [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — qubitization with [[QROM (Quantum Read-Only Memory)|QROM]], alias sampling; established the fault-tolerant electronic structure simulation framework
- **[15]** [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes|Berry, Kieferová, Scherer, Sanders, Low, Wiebe, Gidney, Babbush (2018)]] — improved techniques for preparing eigenstates of fermionic Hamiltonians (Marika's paper)
- **[16]** [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — qubitization / quantum walk framework
- **[17]** [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush (2019)]] — qubitization of arbitrary-basis chemistry; QROAM; measurement-based uncomputation
- **[18]** Loaiza & Izmaylov (2023) — BLISS (block-invariant symmetry shift)
- **[21]** [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma & Boixo (2013)]] — original spectral gap amplification
- **[22]** Low & Chuang (2017) — uniform spectral amplification
- **[23]** Zlokapa & Somma (2024) — [[Hamiltonian simulation]] for low-energy states
- **[25]** Gilyen, Su, Low, Wiebe (2019) — QSVT / quantum singular value transformation
- **[37]** [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes|Reiher et al. (2017)]] — original FeMoCo resource estimate (~$10^{14}$ T gates); this paper achieves ~$300{,}000\times$ improvement
- **[38]** Li et al. (2019) — FeMoCo-76 active space definition
- **[39]** Oumarou et al. (2024) — symmetry compression of double factorization
- **[53]** [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al. (2021)]] — low rank representations / original double factorization

---

## My assessment (as an AI)

This is a genuinely significant paper. The key insight is that the $O(\Lambda/\varepsilon)$ scaling of qubitization-based phase estimation is not a hard wall: if the Hamiltonian has SOS structure (and all fermionic Hamiltonians do), the effective normalization can be brought down to $O(\sqrt{\Lambda E_{\text{gap}}}/\varepsilon)$.

The $4\times$ improvement on FeMoCo-76 is impressive but understates the method's potential. The CO₂ series shows $100\times$+ improvements, and the scaling fits suggest the advantage grows with system size. The DFTHC factorization is also a solid contribution independent of spectrum amplification — the idea that you can smoothly interpolate between DF and THC to hit the Amdahl's-law sweet spot on the circuit cost is elegant.

What I find most interesting is how this connects to the broader story of SOS hierarchies in quantum information. The SOS relaxation for computing $E_{\text{SOS}}$ is exactly the dual of the 2-RDM variational method from quantum chemistry. The quality of the quantum speedup depends on how well classical methods can bound the ground-state energy from below. There's a pleasing irony: the better your classical lower bounds, the bigger your quantum speedup. This creates a productive interplay between classical and quantum methods rather than a competition.

The paper's connection to [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes|SOSSA (2505.01528)]] is direct — that companion paper generalizes the SOS+SA framework beyond electronic structure to arbitrary Hamiltonians and demonstrates it on SYK. This paper is the chemistry-specific version with all the engineering details worked out.

State preparation being comparable in cost to phase estimation is a significant milestone. It means the next frontier is jointly optimizing both components, which [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes|Berry, Tong et al. (2024)]] already started doing from the state-prep side.

---

## Cross-links

### Paper notes
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes]] — foundational spectrum amplification; this paper applies it to chemistry
- [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes]] — general SOSSA theory paper: self-contained framework, adaptive algorithms, SYK demonstration, query-optimal lower bounds
- [[SOSSA — Sum-of-Squares Spectral Amplification]] — informal note on the same general theory paper
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — prior best resource estimates; THC factorization that DFTHC generalizes
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — established the qubitization + QROM + alias sampling framework used here
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — qubitization / quantum walk
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — single/sparse factorization qubitization; QROAM
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — double factorization origin
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]] — Berry, Kieferová et al.; cited as ref [15]
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]] — state preparation now comparable cost to QPE
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — RDM constraints related to the SOS/SDP approach
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — earlier sum-of-squares decomposition for fermion operators
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — resource estimation methodology
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — CPD1-P450 benchmarks; this paper improves on those resource estimates

### Trick cards
- [[Rectangular Block-Encoding for Spectrum Amplification]] — new trick from this paper
- [[SOS Decomposition via Semidefinite Programming for Chemistry]] — new trick from this paper
- [[DFTHC Factorization]] — new trick from this paper
- [[Randomized Error Budget for Phase Estimation]] — new trick from this paper
- [[SOS Spectral Amplification]] — general trick from SOSSA; this paper gives the chemistry-specific implementation
- [[Double Factorisation for Block-Encoding]] — generalized by DFTHC
- [[Coherent Alias Sampling for PREPARE]] — used for state preparation
- [[Frustration-Free Gap Amplification via Walk Operator]] — related concept from Somma-Boixo
- [[Linear Combination of Unitaries (LCU)]] — underlying framework
- [[Oblivious Amplitude Amplification (Robust)]] — used in the T₂ block-encoding construction

- [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes]] — foundational paper introducing uniform spectral amplification and low-energy subspace amplification; this paper applies the idea to chemistry with SOS decompositions

See [[FeMoCo Resource Estimation Timeline]] for how this estimate fits in the broader progression.
