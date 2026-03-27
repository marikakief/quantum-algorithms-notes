> **Source:** Nicholas C. Rubin, Dominic W. Berry, Alina Kononov, Fionn D. Malone, Tanuj Khattar, Alec White, Joonho Lee, Hartmut Neven, Ryan Babbush, Andrew D. Baczewski, *Quantum computation of stopping power for inertial fusion target design*, arXiv:2308.12352, PNAS **121**, e2317772121 (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2308.12352) · [Journal](https://doi.org/10.1073/pnas.2317772121)
> **Tags:** #quantum-simulation #first-quantized #stopping-power #warm-dense-matter #ICF #non-Born-Oppenheimer #resource-estimation #Toffoli-count #product-formula #qubitization #dynamics #plane-wave

---

## The computational problem

Compute the **stopping power** — the rate of kinetic energy loss of a charged projectile (e.g., alpha particle, proton) passing through a target material — from first principles, in the warm dense matter (WDM) regime relevant to inertial confinement fusion (ICF).

The physical setup: $\eta$ electrons and one quantum projectile nucleus (mass $M_{\text{proj}}$, charge $\zeta_{\text{proj}}$) evolve under the non-Born-Oppenheimer Hamiltonian

$$H = H_0 - \frac{\nabla^2_{\text{proj}}}{2M_{\text{proj}}} - \sum_{i=1}^{\eta} \frac{\zeta_{\text{proj}}}{\|r_i - R_{\text{proj}}\|} + \sum_{\ell=1}^{L} \frac{\zeta_{\text{proj}}\zeta_\ell}{\|R_\ell - R_{\text{proj}}\|}$$

where $H_0$ is the standard [[First-Quantized Plane-Wave Chemistry Encoding|first-quantized Born-Oppenheimer Hamiltonian]] for $\eta$ electrons and $L$ classical nuclei. The target nuclei remain classical; the projectile is promoted to a quantum degree of freedom. This avoids time-dependent [[Hamiltonian simulation]] entirely — the resulting $H$ is time-independent.

The observable of interest: the projectile's kinetic energy as a function of time. The stopping power is extracted from the slope of kinetic energy loss vs. displacement.

Input: plane-wave representation of $\eta$ electrons ($n_p$ qubits per spatial component) and one projectile ($n_n$ qubits per component, with $n_n \approx n_p + 3$ due to the projectile's larger momentum-space support). System sizes: 28–1729 electrons at ICF-relevant densities (1–10 g/cm³).

Output: stopping power to precision $\sim 0.1$ eV/Å ($\approx 0.002$ a.u.).

---

## What the paper does

This is the first constant-factor resource estimate for a practically relevant **quantum dynamics** simulation — not eigenvalue estimation, but time evolution with observable readout. It adapts and extends the [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]] first-quantized block encoding to handle a non-BO projectile, then costs the full protocol end-to-end: initial state preparation, time evolution (via both [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]+QSP and high-order [[Trotterized Time Evolution for Chemistry|product formulas]]), kinetic energy measurement, and classical postprocessing.

The headline numbers for the fully converged alpha-in-hydrogen system (218 electrons): $\sim 10^{15}$ Toffoli gates with 8th-order Trotter, $\sim 2 \times 10^{17}$ with QSP, and $\sim 10^3$ logical qubits. That's roughly 100× more Toffolis than FeMoCo-scale ground-state problems, but the same order of logical qubits. A benchmark-scale version (28 electrons) drops to $\sim 10^{13}$ Toffolis.

My assessment: this paper establishes that quantum dynamics simulations of WDM are within reach of the same hardware generation that could tackle molecular ground states — the qubit overhead is modest, and the Toffoli overhead (while large) is polynomial. The [[Product Formulas]] approach wins over QSP by two orders of magnitude here, largely because the numerically optimized 8th-order formula has a tiny prefactor ($\xi = 3.4 \times 10^{-8}$). The paper also contributes a genuinely useful hybrid QROM+Newton-Raphson approach for computing inverse square roots in Trotter steps.

---

## The algorithm / construction

### Protocol overview

Four steps:

1. **Initial state preparation:** Electronic subsystem drawn from a thermal (Mermin Kohn-Sham) Slater determinant via [[Givens Rotation Slater Determinant Preparation|Givens rotation protocol]]; projectile initialized as a Gaussian wave packet in momentum space centered at $k_{\text{proj}}$ with variance $\sigma_k$.

2. **Time evolution:** Under the time-independent non-BO Hamiltonian $H$, using either QSP ([[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]) or high-order [[Product Formulas]]s.

3. **Measurement:** Estimate the projectile kinetic energy $\langle T_{\text{proj}} \rangle$ at $\sim 10$ time points.

4. **Postprocessing:** Linear regression of kinetic energy vs. time gives stopping power.

### Non-BO Hamiltonian in plane waves

The full Hamiltonian (Eq. 2 in the paper) decomposes as:

$$H = T_{\text{elec}} + T_{\text{proj}} + U_{\text{elec}} + U_{\text{proj}} + V_{\text{elec}} + V_{\text{proj}}$$

where $T_{\text{proj}}$ is the projectile kinetic energy (computed in a shifted momentum frame centered on $k_{\text{proj}}$), $U_{\text{proj}}$ is the projectile-classical-nuclei Coulomb interaction, and $V_{\text{proj}}$ is the electron-projectile Coulomb interaction. The projectile uses a larger momentum grid $\tilde{G}$ with $n_n$ qubits per component ($n_n \approx n_p + 3$), needed to resolve a Gaussian wave packet with $\sigma_k \sim 6$–$10$ a.u.

Total qubits for the system: $3\eta n_p + 3n_n$.

### Qubitization approach (QSP)

Extends the [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]] block encoding to include the projectile terms. Key modifications:

- **Three kinetic energy terms** ($T_{\text{elec}}$, $T_{\text{proj}}$, and the cross-term $T_{\text{mean}}$ from the momentum-frame shift) replace the single $T_{\text{elec}}$.
- **New $\lambda$ values** for projectile terms. The total 1-norm becomes:

$$\lambda_H = \max\left[\lambda_T^{\text{elec}} + \lambda_T^{\text{proj}} + \lambda_T^{\text{mean}} + \lambda_U^{\text{elec}} + \lambda_U^{\text{proj}} + \lambda_V^{\text{elec}} + \lambda_V^{\text{proj}},\; \frac{\lambda_U^{\text{elec}} + \lambda_V^{\text{elec}}/(1-1/\eta) + \lambda_V^{\text{proj}}}{p_\nu} + \frac{\lambda_U^{\text{proj}}}{p_{\nu,\text{proj}}}\right]$$

- **Separate $\nu$ preparation** for $U_{\text{proj}}$ (which needs superposition over the larger $\tilde{G}_0$) vs. other potential terms (over $G_0$). The [[Nested Box State Preparation with Overlapping Boxes|nested box preparation]] is made controlled between the two cases at cost $n_n - n_p$ extra Toffolis, using controlled Hadamards.
- **[[Failed PREP Fallback to Alternative Hamiltonian Term|Failed PREP fallback]]** extended to three kinetic terms: on state-preparation failure, a register selects among $T_{\text{elec}}$, $T_{\text{proj}}$, $T_{\text{mean}}$.
- **Controlled swaps** (C4, the dominant cost) increase from $12\eta n_p - 8$ to $12\eta n_p + 6n_n + 4\eta - 6$ Toffolis, because the projectile register must also be swappable into the working register.

QSP synthesizes $e^{-iHt}$ with cost $O(\lambda t + \log(1/\varepsilon)/\log\log(1/\varepsilon))$ queries to the walk operator, each costing $C_{\text{B.E.}}$ Toffolis.

### Product formula approach (8th-order Trotter)

For the [[Product Formulas]], the paper works with a **real-space grid Hamiltonian** (dual to the plane-wave representation) and considers only electronic degrees of freedom (projectile costs are subdominant).

Each Trotter step involves:
1. Computing potential energy in position basis and phasing
2. Computing kinetic energy in momentum basis and phasing
3. QFT between the two bases

The bottleneck is step 1: computing $\eta(\eta-1)/2$ pairwise Coulomb potentials, each requiring an inverse square root $1/\sqrt{x}$.

**[[QROM Interpolation plus Newton-Raphson for Inverse Square Root|Key innovation — hybrid QROM + Newton-Raphson for $1/\sqrt{x}$:]]** Instead of the pure Newton-Raphson iteration of Jones et al. (2012) (which only works over a narrow range), the paper uses:

1. Variable-spacing [[QROM (Quantum Read-Only Memory)|QROM]] to load cubic polynomial interpolation coefficients (15 bits precision). Cost: $4n + 2$ Toffolis where $n$ is the bit size per spatial direction.
2. Evaluate the cubic polynomial via Horner's rule (3 multiplications at $\sim 15^2$ Toffolis each).
3. One step of generalized Newton-Raphson: $y \mapsto \frac{1}{2}y(2 + b^2 - y^2x)$ to compute $b/\sqrt{x}$ directly (absorbing the [[Product Formulas]] coefficient $b$ into the iteration). This avoids a separate multiplication by the Trotter coefficient.

Total per-pair cost: $\sim 2395$ Toffolis (for $n = 6$), given by:

$$2137 + 4n^2 + 19n$$

The generalized Newton-Raphson that computes $b/\sqrt{x}$ instead of $1/\sqrt{x}$ is a nice optimization — it saves one multiplication per pair by folding the product formula coefficient into the iteration. Relative error: $\sim 2.6 \times 10^{-9}$.

**Numerically optimized 8th-order product formula:** A bespoke symmetric formula (Appendix C) found by solving the Yoshida equations over $>$100,000 candidates. The formula has 21 stages ($S_2(w_i t)$ for $i = -10, \ldots, 10$) with 17 exponentials per step.

**Prefactor determination:** The spectral norm $\|S_k(t) - e^{-iHt}\|$ is computed numerically via power iteration on 64-orbital (128-qubit) systems with 2–4 particles, using the Fermionic Quantum Emulator (FQE). The prefactor for the 8th-order formula: $\xi = 3.4 \times 10^{-8}$. This is the number that makes Trotter so competitive here.

Number of Trotter steps:

$$r \approx t^{1+1/k}(\|\tau\|_1 + \|\nu\|_{1,[\eta]})^{1-1/k}(\xi \|\tau\|_1 \|\nu\|_{1,[\eta]}\, \eta / \varepsilon)^{1/k}$$

where $\|\tau\|_1 \approx 3\pi^2 N^{2/3}/(2\Omega^{2/3})$ and $\|\nu\|_{1,[\eta]} \approx \pi^{1/3}(3/4)^{2/3}\eta^{2/3}N^{1/3}/\Omega^{1/3}$.

### Projectile wave packet design

The projectile is initialized as a Gaussian wave packet with momentum-space width $\sigma_k$ and real-space width $\sigma_r = 1/\sigma_k$. Two competing constraints:

- **Small $\sigma_k$** (broad in real space) → easy sampling but the projectile looks less like a point charge, introducing bias
- **Large $\sigma_k$** (narrow in real space) → physical point-charge limit but harder to sample and needs more plane waves

The paper uses TDDFT convergence tests to calibrate: $\sigma_k \approx 5$–$10$ a.u. for low-$Z$ projectiles at ICF-relevant densities. This requires $n_n \approx n_p + 3$ qubits per spatial component for the projectile register.

### Observable estimation

Two strategies compared for estimating $\langle T_{\text{proj}} \rangle$:

1. **Standard Monte Carlo sampling:** Measure in the computational (momentum) basis, compute kinetic energy classically. Variance $\propto \sigma_k^2/(2M_{\text{proj}})$. Requires $N_s \sim 50$–100 circuit repetitions per time point for 0.1 eV/Å precision.

2. **Heisenberg-scaling mean estimation** (Kothari & O'Donnell): $O(\sigma^2/\varepsilon)$ queries instead of $O(\sigma^2/\varepsilon^2)$, but with large constant factors from the Grover-like iterate construction. The paper provides the first constant-factor analysis of the KO algorithm primitives using Cirq-FT.

The crossover: Monte Carlo wins for the moderate precision needed here ($\varepsilon \sim 0.1$ Ha). The KO algorithm would dominate only at higher precision ($\varepsilon \lesssim 0.01$ Ha), relevant for e.g. near-Bragg-peak stopping or other dynamics problems requiring finer resolution.

---

## Key results

### Resource estimates (Table IV in the paper)

| System | $\eta$ | QSP Toffolis | Trotter Toffolis | QSP Qubits | Trotter Qubits |
|---|---|---|---|---|---|
| $\alpha$ + H (50%) | 28 | $5.6 \times 10^{14}$ | $1.1 \times 10^{13}$ | 1,749 | 2,666 |
| $\alpha$ + H (75%) | 92 | $2.0 \times 10^{16}$ | $3.1 \times 10^{14}$ | 3,309 | 3,902 |
| $\alpha$ + H (full) | 218 | $2.0 \times 10^{17}$ | $1.4 \times 10^{15}$ | 5,650 | 6,170 |
| p + D | 1,729 | $2.1 \times 10^{20}$ | $2.1 \times 10^{17}$ | 33,038 | 33,368 |
| p + C | 391 | $2.2 \times 10^{18}$ | $1.1 \times 10^{16}$ | 8,841 | 9,284 |

Parameters: 10 time points from $t=1$ to $t=10$ a.u., infidelity $\varepsilon = 0.01$, $N_s = 50$ samples per time point, $\sigma_k = 6$ a.u.

### Block encoding parameters (Table III)

| System | $C_{\text{B.E.}}$ [Toffolis] | $\lambda$ | Logical qubits |
|---|---|---|---|
| $\alpha$ + H | 24,980 | 1,744,784 | 5,650 |
| p + D | 142,300 | 88,202,785 | 33,038 |
| p + C | 38,360 | 7,727,607 | 8,841 |

The block encoding cost is dominated by controlled swaps (C4) — the same bottleneck as in [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]].

### Key scaling

- QSP: $\tilde{O}(\eta^2/\Delta^2 + \eta^3/\Delta)$ where $\Delta$ is grid spacing
- Trotter: scales as approximately $O(\eta^2)$ at fixed Wigner-Seitz radius with increasing grid resolution
- QPE would cost ~$10\times$ more than time evolution + sampling for the same system, because the QPE precision requirement ($\sim 10^{-3}$) is much more expensive than the stopping power precision requirement

---

## Comparison with prior work

| Aspect | This work | [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]] | Classical TDDFT |
|---|---|---|---|
| Problem | Stopping power (dynamics) | Ground state energy (QPE) | Stopping power (dynamics) |
| System | Non-BO (quantum projectile) | BO (classical nuclei) | Ehrenfest dynamics |
| Electrons | 28–1,729 | 10–500 | 28–1,729 |
| Toffolis | $10^{13}$–$10^{17}$ (Trotter) | $10^{10}$–$10^{12}$ (QPE) | N/A |
| Qubits | $10^3$–$3.3 \times 10^4$ | $10^2$–$10^3$ | N/A |
| Accuracy | Beyond mean-field (exact dynamics) | Beyond mean-field (exact spectrum) | Mean-field only |
| Classical cost | — | — | $\sim 10^8$ CPU-hours/year |

The $\sim 100\times$ Toffoli increase over FeMoCo-scale QPE comes from: (a) time evolution cost scaling with $\lambda t$ vs. $\lambda/\varepsilon$, (b) 10 time points × 50 samples = 500× multiplier, and (c) much larger $\lambda$ from larger systems.

---

## Limits / caveats

1. **Product formula bounds are loose.** The spectral norm bound from power iteration on small systems ($\eta = 2$–4) is extrapolated to $\eta = 218$–1729. The actual Trotter error could be significantly smaller for physical states (average-case vs. worst-case).

2. **Thermal state preparation isn't costed.** The protocol assumes a Mermin Kohn-Sham Slater determinant from a classical DFT calculation. Preparing a more accurate thermal state would add cost.

3. **Projectile remains essentially classical.** The Gaussian wave packet is designed to approximate a point charge throughout the dynamics. The non-BO treatment is a computational convenience (avoiding time-dependent [[Hamiltonian simulation]]), not a physically motivated choice.

4. **No error budget for discretization.** The plane-wave cutoff and $\sigma_k$ choices are calibrated against TDDFT convergence, not against exact benchmarks.

5. **The 50 samples estimate** for thermal averaging is a rough number. A full assessment would require running many quantum dynamics trajectories with thermally distributed initial conditions.

6. **Proton-in-deuterium is enormous.** At 1,729 electrons, 33,038 qubits, and $2 \times 10^{17}$ Toffolis, the fully converged system is well beyond near-term capabilities. The 28-electron benchmark system is the realistic first target.

---

## Reusable ideas

1. [[Non-BO Projectile Block Encoding Extension]] — Extending first-quantized plane-wave block encodings to handle a quantum projectile with a different (larger) momentum grid than the electrons. The controlled preparation trick for the $\nu$ superposition costs only $n_n - n_p$ extra Toffolis.

2. [[QROM Interpolation plus Newton-Raphson for Inverse Square Root]] — Hybrid function evaluation: QROM loads a cubic polynomial interpolant at 15-bit precision, then one Newton-Raphson step boosts to $\sim 10^{-9}$ relative error. Generalizing to $b/\sqrt{x}$ folds the [[Product Formulas]] coefficient into the iteration for free.

3. [[Gaussian Wave Packet Variance Tuning for Observable Estimation]] — Designing the projectile wave packet width $\sigma_k$ to balance physical accuracy (point-charge approximation) against sampling efficiency. Calibrated via classical TDDFT convergence tests.

4. [[Shot-Noise vs Heisenberg-Scaling Observable Estimation Crossover]] — At moderate precision ($\varepsilon \sim 0.1$ Ha), standard Monte Carlo sampling beats the Kothari-O'Donnell Heisenberg-scaling algorithm due to the latter's large constant factors. The crossover point depends on the specific observable and precision target.

---

## References within this paper

- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] — Primary algorithmic foundation. This paper extends Su et al.'s block encoding to non-BO systems.
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven (2019)]] — First-quantized plane-wave framework; interaction picture approach discussed but not used here.
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]] — Source of the QROM function interpolation technique used for the inverse square root.
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes|Babbush, Huggins, Berry et al. (2023)]] — Establishes the asymptotic advantage of quantum dynamics over classical mean-field methods; this paper applies that insight to a concrete problem.
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry, Rubin, Babbush et al. (2024)]] — Contemporaneous work on first-quantized materials simulation with pseudopotentials.
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]] — Second-quantized materials simulation comparison point.
- Kothari and O'Donnell (2023) — Mean estimation with Heisenberg scaling; paper provides first constant-factor analysis of this algorithm's cost for a concrete problem.
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al. (2020)]] — Trotter simulation of condensed-phase systems; norm scaling analysis.
- Morales, Costa, Burgarth, Sanders, Berry (2022) — Numerically optimized higher-order [[Product Formulas]]s; the bespoke 8th-order formula used here builds on this approach.
- Jones et al. (2012) — Original Newton-Raphson approach for computing inverse square root on quantum computers; this paper improves on it with the QROM hybrid.

---

## Cross-links

### Paper notes
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — Direct predecessor; this paper extends Su et al.'s block encoding
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — Asymptotic advantage argument that motivates this work
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — Source of QROM interpolation
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — Related first-quantized materials work
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — Trotter for condensed-phase systems
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Original qubitization framework
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]] — the Stopping Power paper's block encoding primitives are among the non-BO first-quantized circuits implemented in Qualtran; Rubin is coauthor on both

### Trick cards
- [[First-Quantized Plane-Wave Chemistry Encoding]] — The representation this paper builds on
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — One of the two time-evolution methods
- [[Trotterized Time Evolution for Chemistry]] — The other time-evolution method
- [[Failed PREP Fallback to Alternative Hamiltonian Term]] — Extended here to three kinetic terms
- [[Kinetic Energy as Bit-Product LCU]] — Used for kinetic SELECT in the qubitization approach
- [[QROM (Quantum Read-Only Memory)]] — Used for nuclear position loading and inverse square root interpolation
- [[Controlled Swap Network for Select Oracle]] — Dominant cost in the block encoding
- [[Givens Rotation Slater Determinant Preparation]] — Used for initial electronic state preparation
- [[Coherent Alias Sampling for PREPARE]] — Used in PREPARE circuits
- [[Adaptive QROM for Function Evaluation]] — Related to the QROM interpolation approach
- [[Non-BO Projectile Block Encoding Extension]] — New trick card from this paper
- [[QROM Interpolation plus Newton-Raphson for Inverse Square Root]] — New trick card from this paper
- [[Gaussian Wave Packet Variance Tuning for Observable Estimation]] — New trick card from this paper
- [[Shot-Noise vs Heisenberg-Scaling Observable Estimation Crossover]] — New trick card from this paper
- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]] — Follow-up that resolves the $O(\eta^2)$ Coulomb bottleneck in this paper's [[Product Formulas]] approach via quantum FMM
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — systematic search and comparison of high-order [[Product Formulas]]s; the bespoke 8th-order formula used in this paper is from the same research programme
