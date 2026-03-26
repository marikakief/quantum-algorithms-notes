> **Source:** Dominic W. Berry, Yu Tong, Tanuj Khattar, Alec White, Tae In Kim, Sergio Boixo, Lin Lin, Seunghoon Lee, Garnet Kin-Lic Chan, Ryan Babbush, Nicholas C. Rubin, *Rapid initial state preparation for the quantum simulation of strongly correlated molecules*, arXiv:2409.11748 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2409.11748)
> **Tags:** #quantum-simulation #quantum-chemistry #state-preparation #MPS #phase-estimation #window-functions #FeMoCo #fault-tolerant #resource-estimation #iron-sulfur #unitary-synthesis

---

## The computational problem

Given a molecular Hamiltonian $H$ with ground state $|\Psi_0\rangle$ and ground state energy $E_0$, estimate $E_0$ to precision $\varepsilon$ with confidence level $1 - q$. The full cost includes: (1) preparing an initial state $|\Phi\rangle$ with squared overlap $p = |\langle \Phi | \Psi_0 \rangle|^2$, and (2) filtering via [[Qubitization (Quantum Walk for Spectral Encoding)|quantum phase estimation]] to extract $E_0$ from the spectrum. Both costs depend on $p$, the block encoding normalisation $\lambda$, the target error $\varepsilon$, and the confidence level $1 - q$.

This is the paper that actually closes the loop on FeMoCo resource estimation by addressing the two pieces that prior work (especially [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. 2021]]) left open: *how* to prepare a high-overlap initial state, and *how many repetitions* of QPE are needed given imperfect overlap.

---

## What the paper does

Three main contributions:

1. A new unitary synthesis method that reduces Toffoli cost by $\sim 7\times$ over the LKS method (Low, Kliuchnikov, Schaeffer 2024), applied to MPS state preparation.
2. Two filtering approaches — sampling and binary search — for extracting the ground state energy from QPE with imperfect overlap, both optimised using window functions (Kaiser and Slepian prolate spheroidal).
3. End-to-end resource estimates for Fe-S clusters including FeMoCo, using an MPS overlap extrapolation protocol that estimates $|\langle \text{MPS}(M) | \Psi_\infty \rangle|^2$ from finite bond dimension data.

The bottom line for FeMoCo: $7.3 \times 10^{10}$ Toffoli gates (THC, 95% confidence), which is only $2.3\times$ the cost in [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]] that assumed perfect overlap. This is surprisingly modest — the state preparation problem, which [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al. (2022)]] identified as the elephant in the room, turns out to be manageable for FeMoCo when using MPS initial states with sufficient bond dimension.

My assessment: this is one of the most practically important papers in the quantum chemistry simulation pipeline. It converts the abstract "assume good overlap" placeholder into concrete numbers. The $7\times$ unitary synthesis improvement is technically nice but the real value is showing that MPS preparation + a couple of QPE repetitions is enough for FeMoCo. The overlap extrapolation is empirical (not rigorous), which is the main caveat.

---

## The algorithm / construction

### Part 1: MPS state preparation via improved unitary synthesis

An MPS with $N$ sites, physical dimension $d$, and bond dimension $\chi$ can be prepared using the Schön-Solano-Verstraete-Cirac-Wolf (SSCVW) protocol: apply a sequence of unitaries $G[j]$ on $d\chi$-dimensional registers, each acting on the $\chi$-dimensional ancilla and one physical subsystem.

The key insight is that the MPS left-canonical form means only $\chi$ columns of each $d\chi \times d\chi$ unitary need to be correctly synthesised (the input on the physical leg is always $|0\rangle$).

#### New unitary synthesis via rectangular multiport decomposition

The method decomposes an $N_{\text{un}} \times N_{\text{un}}$ unitary using the rectangular decomposition of Clements et al. (2016), reinterpreted as alternating layers of:
- Diagonal phase rotations (loaded via [[QROM (Quantum Read-Only Memory)|QROM]])
- Hadamard gates on a single qubit
- Increment/decrement operations (to shift between even and odd layer block structures)

Each beam splitter in the multiport interferometer decomposes as two 50/50 beam splitters (Hadamards) with phase shifts between them. The phases from adjacent layers can be absorbed into each other, leaving at most $N_{\text{un}} + 1$ QROM lookups each outputting $N_{\text{un}}$ phase values.

The QROM cost for each layer is:

$$\left\lfloor \frac{N_{\text{un}}}{2\Lambda} \right\rfloor + (2\Lambda b - 5)$$

where $\Lambda$ is a power of 2 (QROM branching factor) and $b$ is the rotation bit precision. The QROM erasure sign fixup can be folded into subsequent phase layers at zero cost — a nice trick.

Total cost for the full unitary: $N_{\text{un}}$ QROM calls plus $(n-2)(N_{\text{un}} - 1)$ Toffolis for increment/decrement operations.

#### Column synthesis (the half-columns trick)

When only $N_{\text{un}}/d$ columns are needed, the problem reduces via SVD/QR decomposition:

$$\begin{pmatrix} A & ? \\ B & ? \end{pmatrix} = \begin{pmatrix} U_1 & 0 \\ 0 & U_2 \end{pmatrix} \begin{pmatrix} D_1 & ? \\ D_2 & ? \end{pmatrix} \begin{pmatrix} V & 0 \\ 0 & V \end{pmatrix}$$

where $D_1, D_2$ are diagonal. This decomposes the problem into:
1. A unitary of dimension $N_{\text{un}}/2$ (can be merged with the previous MPS step)
2. A controlled qubit rotation (one QROM + rotation)
3. Controlled unitaries on the two subspaces (half the layers of the full unitary)

For general $d$, iterate the procedure $d - 1$ times (linear in $d$, vs. $\sqrt{d/2}$ for LKS — so LKS wins for $d \gtrsim 30$, but for the typical chemistry case $d = 4$, the new method wins by $\sim 3.5\times$).

#### Concrete MPS preparation costs

For FeMoCo with MPS bond dimension $\chi = 4000$ ($d = 4$, 76 spatial orbitals): 733 million Toffolis and 682 qubits. For $\chi = 6000$: 1.36 billion Toffolis and 833 qubits. Computed using [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Qualtran]].

### Part 2: Phase estimation with window functions

The QPE control register is initialised in a state $|\Gamma\rangle = \sum_n \gamma_n |n\rangle$ where the $\gamma_n$ are samples of a window function $w(z)$. After QFT, the probability of getting phase estimate $\hat{\phi}$ is determined by the kernel $\Gamma(x) = \frac{1}{\sqrt{2N}} \sum_n e^{inx} \gamma_n$.

**[[Kaiser Window Amplitude Estimation|Kaiser window]]:** $w(x) = I_0(\pi\alpha\sqrt{1 - (x/N)^2})$, giving error distribution proportional to $\mathrm{sinc}^2(\sqrt{N^2\theta^2 - \pi^2\alpha^2})$. The tail probability is:

$$\delta \approx 4\ln(2\alpha)\sqrt{\alpha}\, e^{-2\pi\alpha}$$

The paper provides higher-order corrections using modified Bessel and Struve functions, giving:

$$\delta = \frac{4C_\alpha}{\sqrt{\alpha}} e^{-2\pi\alpha}\left(1 - \frac{5}{16\pi\alpha} + O(\alpha^{-2})\right)$$

where $C_\alpha = \ln(2\alpha) + \mathrm{Ci}(2\pi)$.

**Prolate spheroidal window (optimal):** The angular spheroidal function $\mathrm{PS}_{0,0}(c, z)$. The paper corrects a 1965 error in Slepian's higher-order expansion:

$$\delta = 4\sqrt{\pi c}\, e^{-2c}\left(1 - \frac{7}{16c} - \frac{91}{2^9 c^2} + O(c^{-3})\right)$$

(Slepian had $-3/(32c)$ for the first correction term — wrong by a factor of $\sim 2$.)

Despite the optimal window giving asymptotically smaller error, the cost $N$ is the same to leading order:

$$N = \frac{\ln(1/\delta)}{2\varepsilon} + O\left(\frac{\ln\ln(1/\delta)}{\varepsilon}\right)$$

The practical difference is negligible — the prolate spheroidal window removes only a triple-logarithmic term.

### Part 3: The sampling method

Take the minimum energy estimate over $n$ QPE samples. Two error types:
- **Type I:** An excited state yields an estimate more than $\varepsilon$ below $E_0$ (catastrophically low estimate)
- **Type II:** All estimates correspond to excited states (no ground state sample)

The total error probability is bounded by:

$$P_{\text{err}} = [1 - p(1 - \delta/2)]^n + 1 - (1 - \delta/2)^n \leq q$$

The optimal number of samples is slightly above $(1/p)\ln(1/q)$. Solving for $\delta$ and optimising gives total query complexity:

$$\frac{n\lambda}{2\varepsilon}\ln(1/\delta) \approx \frac{\lambda\ln(2/q)}{2p\varepsilon}\ln\left(\frac{\ln(2/q)}{pq}\right)$$

An important finding: accounting for the interplay between Type I and Type II errors from excited states, the Kaiser window actually *beats* the prolate spheroidal window. The Kaiser window more strongly suppresses tails, reducing the probability of excited-state measurements producing catastrophically low estimates. This matters when $p$ is small and the number of samples is large.

### Part 4: The binary search method

For small overlap ($p \lesssim 0.003$), a binary search with amplitude estimation provides better scaling. At each step, shrink the candidate energy interval by factor $\omega = 1/\sqrt{2}$ (optimised from the generic $2/3$). Each step solves a "fuzzy bisection" problem using amplitude estimation on the QPE circuit.

Total query complexity to $W$:

$$\frac{7.77\lambda}{\sqrt{p}\,\varepsilon}\ln\left(\frac{4}{\sqrt{p}}\right)\ln\left(\frac{\log_{\sqrt{2}}(\lambda/\varepsilon)}{q}\right)$$

The $1/\sqrt{p}$ scaling (vs. $1/p$ for sampling) is the square root improvement from amplitude amplification. The constant factor $\sim 7.77$ is about $16\times$ larger than the sampling method's leading constant, so the crossover is at $p \approx 0.003$.

### Part 5: Overlap extrapolation protocol

Two empirically observed linear relations:

$$\log(1 - |\langle\Phi(M')|\Phi(\infty)\rangle|^2) \quad \text{vs.} \quad (\log M')^2$$

$$\log(|\langle\Phi(M')|\Phi(M'')\rangle|^2 - |\langle\Phi(M')|\Phi(\infty)\rangle|^2) \quad \text{vs.} \quad (\log M'')^2 \quad (M' \ll M'')$$

By fitting these relations for small $M'$ values and extrapolating, the protocol estimates the overlap at any target bond dimension. Validated on Fe$_2$S$_2$ and Fe$_4$S$_4$ where exact ground states are accessible.

### Part 6: Symmetry shifting for 1-norm reduction

The LCU 1-norm $\lambda$ is reduced by shifting $H \to H - (\alpha_2/2)\hat{N}^2 - (\alpha_1 - \alpha_2/2)\hat{N}$ where $\hat{N}$ is the electron number operator. This exploits the known electron number to cancel part of the two-body repulsion. For FeMoCo: $\lambda_{\text{THC}}$ drops from 1201.5 (Lee et al. 2021) to 781.8, a factor of $\sim 1.54$ reduction.

---

## Key results

**MPS preparation costs:**

| System | Bond Dimension | Estimated overlap $|\langle \text{MPS}|\psi_0\rangle|$ | MPS Toffolis | Qubits |
|---|---|---|---|---|
| Fe$_2$(III)Fe$_2$(II) | 1000 | 0.88 | 42.2M | 359 |
| Fe$_4$(III) | 1000 | 0.92 | 42.2M | 359 |
| FeMoCo MPS1 | 6000 | 0.99 | 1.36B | 833 |
| FeMoCo MPS2 | 4000 | 0.95 | 733M | 682 |
| FeMoCo MPS3 | 4000 | 0.98 | 733M | 682 |

**Total QPE + MPS costs (confidence intervals of half-width $\varepsilon = 10^{-3}$ Hartree):**

| System | Method | $\lambda$ | BE Toffolis | Qubits | QPE Cost 95% | QPE Cost 99% |
|---|---|---|---|---|---|---|
| Fe$_2$(III)Fe$_2$(II) | THC | 168.7 | 9,120 | 1,149 | $1.33 \times 10^{10}$ | $2.45 \times 10^{10}$ |
| Fe$_2$(III)Fe$_2$(II) | DF | 154.7 | 15,545 | 3,111 | $2.08 \times 10^{10}$ | $3.82 \times 10^{10}$ |
| Fe$_4$(III) | THC | 164.1 | 8,573 | 1,081 | $8.37 \times 10^9$ | $1.67 \times 10^{10}$ |
| FeMoCo | THC | 781.8 | 16,923 | 2,194 | $7.27 \times 10^{10}$ | $1.38 \times 10^{11}$ |
| FeMoCo | DF | 582.4 | 35,006 | 6,402 | $1.11 \times 10^{11}$ | $2.11 \times 10^{11}$ |

**Key numbers for FeMoCo THC at 95% confidence:** $7.3 \times 10^{10}$ Toffoli gates, 2,194 logical qubits. Only 2 QPE samples needed (due to high overlap ~0.9 squared).

---

## Comparison with prior work

| Aspect | This paper | Lee, Berry et al. 2021 (THC) | Lee, Babbush, Chan et al. 2022 |
|---|---|---|---|
| Overlap assumption | MPS with empirical extrapolation | Perfect overlap | Exponential decay for product states |
| FeMoCo Toffolis (THC) | $7.3 \times 10^{10}$ | $3.2 \times 10^{10}$ | — |
| $\lambda$ (THC, Li Hamiltonian) | 781.8 (symmetry shifted) | 1201.5 | — |
| MPS prep cost | 733M–1.36B Toffolis | Not costed | — |
| QPE samples needed | 2 | 1 (assumed) | — |
| Unitary synthesis | $\sim 7\times$ better than LKS | Uses LKS | — |

The $2.3\times$ cost increase over Lee et al. 2021 comes from three factors: (1) symmetry shifting saves $1.54\times$ on $\lambda$, (2) needing 2 QPE samples doubles cost, (3) confidence intervals are more demanding than RMS error. These roughly balance, leaving the total close to the "perfect overlap" estimate.

The paper's result effectively rebuts the concern raised in [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al. (2022)]] that state preparation could be a show-stopper for FeMoCo. MPS bond dimension $\chi = 4000$ gives $\sim 0.9$ squared overlap, and this modest overlap is enough to keep total cost within a small constant factor of the ideal case.

---

## Limits / caveats

1. **Overlap extrapolation is empirical.** The two linear relations used for extrapolation are observed numerically in Fe$_2$S$_2$ and Fe$_4$S$_4$ but have no rigorous justification. If the extrapolation is wrong for FeMoCo, the actual overlap could be lower and costs would increase.

2. **FeMoCo ground state identity is unknown.** The MPS initial states may correspond to different spin-coupled eigenstates, not the true ground state. The paper handles this by preparing multiple candidate MPS states and using QPE to resolve their energies. But this multiplies the cost — the 35 BS-DFT derived candidates would give a $35\times$ overhead in the worst case (reducible by pre-filtering on DMRG extrapolated energies).

3. **MPS bond dimension scaling with system size.** For FeMoCo ($\chi = 4000$) the overlap is good. But the scaling of required $\chi$ with system size for more complex systems is not established. The bond dimension needed for $O(1)$ overlap could grow exponentially for some problem classes.

4. **Binary search overhead.** The binary search approach has a $16\times$ larger constant than sampling, so it only helps for $p < 0.003$. For the high-overlap regime relevant to chemistry with MPS initial states, sampling always wins.

5. **No rigorous guarantee on the DMRG energy ordering.** At finite bond dimension, the energy ordering of competing spin states can differ from the true ordering. QPE resolves this, but only after running the full algorithm for each candidate.

---

## Reusable ideas

1. [[Rectangular Multiport Unitary Synthesis]] — Decompose an $N$-dimensional unitary into alternating layers of QROM-loaded phase rotations and Hadamards, achieving $\sim 7\times$ reduction in Toffoli count over LKS.

2. [[Column Synthesis via SVD-QR Decomposition]] — When only a fraction of a unitary's columns are needed, reduce via SVD/QR to controlled operations on subspaces, halving the number of required QROM layers per reduction step.

3. [[Window Function Optimisation for QPE Confidence Intervals]] — Use Kaiser or prolate spheroidal windows for the QPE control register to minimise confidence interval width at given cost, with asymptotic cost $N = \frac{\ln(1/\delta)}{2\varepsilon} + O(\varepsilon^{-1}\ln\ln(1/\delta))$.

4. [[MPS Overlap Extrapolation Protocol]] — Estimate the overlap of a finite bond dimension MPS with the exact ground state via two empirical linear relations in $(\log M)^2$, without access to the true ground state.

5. [[Number Operator Symmetry Shifting for LCU 1-Norm Reduction]] — Shift the Hamiltonian by $(\alpha_2/2)\hat{N}^2 + (\alpha_1 - \alpha_2/2)\hat{N}$ to reduce the LCU 1-norm $\lambda$ by up to $\sim 2\times$, exploiting known particle number.

---

## References within this paper

- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. (2021)]]: THC qubitization — this paper's block encoding costs for FeMoCo build directly on these methods. The $\lambda$ values and Toffoli costs per block encoding are from Lee et al., modified by symmetry shifting.
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al. (2022)]]: The EQA paper that raised the state preparation concern this work addresses.
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman et al. (2018)]]: [[ASCI Overlap Estimation for QPE Initial State|ASCI overlap estimation]] — earlier approach to bounding the state preparation problem.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]]: Original [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] paper; the controlled walk operator and [[Unary Iteration|unary iteration]] machinery used here.
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney et al. (2019)]]: Single factorization qubitization, QROAM — this paper uses their QROM costing formulae.
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar et al. (2024)]]: MPS preparation Toffoli counts computed using [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Qualtran]].
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry et al. (2020)]]: Explains the $b - 2$ cost for phase rotations into a phase gradient register.
- [[Kaiser Window Amplitude Estimation]]: Berry, Su et al. (2024), PRX Quantum — the Kaiser window analysis this paper extends and improves upon.
- Low, Kliuchnikov, Schaeffer (LKS), Quantum 8, 1375 (2024): Prior unitary synthesis method that this paper improves by $7\times$.
- Schön, Solano, Verstraete, Cirac, Wolf, PRL 95, 110503 (2005): Original MPS preparation protocol using ancilla-based unitary sequences.
- Clements, Humphreys, Metcalf, Kolthammer, Walmsley, Optica 3, 1460 (2016): Rectangular multiport decomposition that inspires the unitary synthesis method.
- Slepian, J. Math. Phys. 44, 99 (1965): Prolate spheroidal wave functions — this paper corrects Slepian's higher-order error terms.
- Imai and Hayashi, NJP 11, 043034 (2009): Analysis of prolate spheroidal window for phase estimation.
- Lin and Tong, Quantum 4, 372 (2020): Binary search approach for ground state energy estimation — the binary search method builds on this.
- Fomichev et al., arXiv:2310.18410 (2023): Prior MPS preparation costing using LKS — this paper achieves $7\times$ improvement.
- Li, Guo, Sun, Chan, Nature Chemistry 11, 1026 (2019): FeMoCo active space Hamiltonian (Li Hamiltonian).
- Li, Chan, JCTC 13, 2681 (2017): DMRG initialisation procedure for FeMoCo.
- Loaiza, Khah, Wiebe, Izmaylov, QST 8, 035019 (2023): Number operator symmetry shifting for 1-norm reduction.

---

## Cross-links

### Paper notes
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — provides the THC block encoding costs this paper inherits; total QPE cost here is $2.3\times$ the Lee et al. estimate that assumed perfect overlap
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — raised state preparation as the key unresolved blocker for FeMoCo QPE; this paper directly resolves it via MPS initial states
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — prior approach to bounding initial-state overlap via ASCI; this paper supersedes it with MPS states that achieve $\sim 0.9$ squared overlap for FeMoCo at $\chi = 4000$, far exceeding what product-state methods achieve
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — original qubitization machinery whose controlled walk operators and unary iteration underpin the block encodings used here
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — QROAM and single-factorization block encodings; QROM costing formulas directly used for MPS preparation costs
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — the original FeMoCo benchmark (~$10^{14}$ T gates via Trotter) that defined the problem this paper finally resolves end-to-end with state preparation
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]] — all MPS preparation Toffoli counts in this paper are computed using Qualtran's bloq framework; Rubin is coauthor on both
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — explains the $b - 2$ Toffoli cost for phase rotations into a phase gradient register, directly used in the unitary synthesis cost model
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — pharmaceutical-chemistry resource estimation target; the MPS initial state approach here is the missing piece for making CYP P450 QPE concrete
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — Rubin and Berry coauthor both; Bloch orbital periodic materials represent the next frontier for MPS state preparation beyond molecular systems
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — double factorization ($\lambda_{\text{DF}} = 582.4$) is one of the two block-encoding strategies compared for FeMoCo; Chan is coauthor on both, linking the integral factorization and initial state communities
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]] — hardware experiments on correlated Fe-S systems; the classical DMRG/MPS methods validated on these systems inform the MPS overlap extrapolation protocol used here

### Trick cards
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[QROM (Quantum Read-Only Memory)]]
- [[QROAM (Space-Time Tradeoff for QROM)]]
- [[Unary Iteration]]
- [[Coherent Alias Sampling for PREPARE]]
- [[Kaiser Window Amplitude Estimation]]
- [[ASCI Overlap Estimation for QPE Initial State]]
- [[Sequential Multi-Determinant State Preparation]]
- [[EQA Assessment Framework (State Preparation vs Classical Heuristic)]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Phase Gradient State for Controlled Rotations]]
- [[Rectangular Multiport Unitary Synthesis]]
- [[Column Synthesis via SVD-QR Decomposition]]
- [[Window Function Optimisation for QPE Confidence Intervals]]
- [[MPS Overlap Extrapolation Protocol]]
- [[Number Operator Symmetry Shifting for LCU 1-Norm Reduction]]

See [[FeMoCo Resource Estimation Timeline]] for how this estimate fits in the broader progression.
