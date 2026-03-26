> **Source:** Dominic W. Berry, Nicholas C. Rubin, Ahmed O. Elnabawy, Gabriele Ahlers, A. Eugene DePrince III, Joonho Lee, Christian Gogolin, Ryan Babbush, *Quantum Simulation of Realistic Materials in First Quantization Using Non-local Pseudopotentials*, arXiv:2312.07654, npj Quantum Information (2024)
> **Links:** [arXiv](https://arxiv.org/abs/2312.07654) · [Journal](https://www.nature.com/articles/s41534-024-00896-9)
> **Tags:** #quantum-simulation #materials #first-quantized #plane-wave #pseudopotential #block-encoding #qubitization #resource-estimation #fault-tolerant #heterogeneous-catalysis #non-cubic

---

## The computational problem

Estimate ground-state energies of periodic materials described by the electronic structure Hamiltonian in a [[First-Quantized Plane-Wave Chemistry Encoding|first-quantized plane-wave basis]], using the Goedecker-Teter-Hutter (GTH) norm-conserving pseudopotential to eliminate core electrons and reduce the required plane-wave cutoff. The Hamiltonian is:

$$H = T + U_{\text{loc}} + U_{\text{nonloc}} + V$$

where $T$ is the kinetic energy (diagonal in momentum), $U_{\text{loc}}$ is the local pseudopotential, $U_{\text{nonloc}}$ is the nonlocal pseudopotential with angular momentum projectors and Legendre polynomials, and $V$ is the electron-electron Coulomb interaction. The system has $\eta$ electrons, $L$ nuclei, and $N = N_x N_y N_z$ plane-wave basis functions stored in $\eta$ registers of $n_x + n_y + n_z$ qubits each.

The non-cubic unit cell is handled through general Bravais vectors with a reciprocal lattice [[Gramian-Based Momentum Norm for Non-Cubic Cells|Gramian]] $g_{ij} = (g^{(i)})^T g^{(j)}$, so the squared norm of a basis vector is $\|k\|^2 = \sum_{ij} g_{ij} p_i p_j$ rather than just a sum of squares.

---

## What the paper does

This is the first complete block encoding of a realistic pseudopotential Hamiltonian in first quantization. Prior work by [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry et al. (2021)]] compiled first-quantized plane-wave chemistry to explicit fault-tolerant circuits, but used a bare Coulomb nuclear potential $U = -4\pi Z_\ell / (\Omega \|k_\nu\|^2)$. That's fine for hydrogen — but real materials have heavy atoms whose core electrons demand enormous plane-wave cutoffs. Pseudopotentials fix this by replacing the core potential with a smooth effective potential, but the GTH pseudopotential has a complicated functional form involving exponentials, polynomials, angular momentum projectors, and Legendre polynomials. The question was whether this complexity would destroy the $\tilde{O}(\eta)$ scaling advantage of first quantization.

The answer: no. The block encoding cost remains $O(10^4)$ Toffolis for systems with 100–500 electrons, roughly the same order as without pseudopotentials. The key insight is to use [[Shared Exponential Evaluation Across Pseudopotential Terms|coherent arithmetic]] rather than [[QROM (Quantum Read-Only Memory)|QROM]] for the functional evaluation. Where Shokrian Zini et al. (2023) loaded the pseudopotential's functional variation via QROM (scaling with total basis size $N$), this paper computes it on the fly using arithmetic (scaling as $O(\log^2 N)$). That's an exponential improvement in the pseudopotential evaluation cost.

The paper also handles non-cubic unit cells properly — allowing different grid resolutions in each Miller direction — and demonstrates the approach on heterogeneous catalysis (CO adsorption on Pt, Pd, Rh) and battery cathode materials (LiNiO₂). The resource estimates are sobering: $O(10^{14})$ Toffolis for phase estimation, dominated by the large $\lambda_{\text{nonloc}}$ values ($O(10^7)$).

My assessment: this is an important engineering paper that makes first-quantized plane-wave simulation applicable to real materials for the first time. The algorithmic innovation (arithmetic over QROM, shared exponential) is elegant and the motivation is spot-on. The comparison with second-quantized methods ([[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin et al. 2023]]) is honest: first quantization costs more Toffolis than second quantization for LNO, but requires ~50× fewer qubits (1,400 vs 75,000). The spacetime volume comparison favours first quantization. The $\lambda$ problem — $\lambda_{\text{nonloc}}$ dominating and being $O(10^7)$ — is the elephant in the room, and the authors are upfront about it.

---

## The algorithm / construction

### Block encoding architecture

The block encoding follows the PREPARE/SELECT framework from [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]], with substantial modifications to accommodate the pseudopotential:

1. **Two qubits** select between $T$, $U_{\text{loc}}$, $U_{\text{nonloc}}$, and $V$.
2. A **superposition over nested boxes** indexed by $\mu$ prepares the momentum transfer $\nu$, with different weighting for $V$ (amplitude $\propto 1/\|k_\nu\|$) and $U$ (box-level upper bounds on pseudopotential functions).
3. **Controlled swaps** bring the relevant electron momentum registers into working ancillae (cost: $12\eta n$ Toffolis via [[Unary Iteration|unary iteration]]).
4. The **nuclear phase** $e^{-ik_\nu \cdot R_\ell}$ is applied via QROM for $R_\ell$ and multiplication into a phase gradient register.

### The pseudopotential block encoding

The central technical problem: the nonlocal pseudopotential depends on *both* $q$ (the system momentum) and $\nu$ (the momentum transfer), through products of exponentials, polynomials, and Legendre polynomials:

$$u^{\alpha}_{\text{non}}(k_p, k_q) = \frac{1}{\Omega} \sum_l \frac{(-1)^l(2l+1)}{4\pi} P_l\!\left(\frac{k_p \cdot k_q}{\|k_p\|\|k_q\|}\right) \sum_{i,j} E^{ij}_{l\alpha} F^i_{l\alpha}(\|k_p\|) F^j_{l\alpha}(\|k_q\|)$$

where $F^i_{l\alpha}(\|k\|) = \|k\|^l C^{\alpha}_{li} \left(\sum_x c_{x,li} (r^\alpha_l \|k\|)^{2x}\right) e^{-(r^\alpha_l \|k\|)^2/2}$.

**Key design decisions:**

1. **Break up the sum over $l, i, j$** in PREPARE rather than computing the full sum in SELECT. This increases $\lambda$ (because you're summing absolute values of individual terms rather than taking the absolute value of the sum), but dramatically reduces the arithmetic cost.

2. **Use box-level upper bounds** $\Psi_{\varsigma,\alpha,\mu,l,i,j}$ for the amplitude in each nested box, rather than computing the exact $\nu$-dependent amplitude. This further simplifies PREPARE at the cost of inflating $\lambda$.

3. **[[Shared Exponential Evaluation Across Pseudopotential Terms|Share the exponential evaluation]]** between local and nonlocal pseudopotentials. The argument $(r^\alpha_l \|k_q\|)^2 + (r^\alpha_l \|k_p\|)^2$ is computed once via [[QROM Interpolation for Negative Exponential|QROM interpolation]], then the same value is used for all $l, i, j$ terms. For the local pseudopotential, setting $q = 0$ gives the correct argument $(r^\alpha_{\text{loc}} \|k_\nu\|)^2$.

4. **Combine the Legendre polynomial with the power of $\|k\|$** to avoid computing ratios. For $l = 0$: trivial (Legendre = 1). For $l = 1$: $P_1(x) \cdot \|k_p\| \|k_q\| = k_p \cdot k_q$. For $l = 2$: $P_2(x) \cdot (\|k_p\| \|k_q\|)^2 = \frac{1}{2}[3(k_p \cdot k_q)^2 - \|k_p\|^2 \|k_q\|^2]$.

5. **Implement the amplitude factor** via inequality testing with an equal superposition register multiplied by $\Psi$, using a controlled-Z for the sign.

### QROM interpolation of the exponential

The negative exponential is the most expensive function to compute. The paper uses a [[QROM Interpolation for Negative Exponential|clever interpolation scheme]]:

1. Multiply argument by $1/\ln 2$ (cost: $\sim b^2/4$ Toffolis since half the bits of $1/\ln 2$ are zero).
2. Split into integer part and fractional part.
3. QROM on leading fractional bits outputs polynomial coefficients for interpolation.
4. Evaluate the interpolating polynomial on the remaining fractional bits.
5. Use the integer part to control a bit shift of the output.

Linear interpolation with 256 points gives relative error $< 10^{-6}$ at Toffoli cost $\sim 256 + (3/4)b^2$. Quadratic interpolation with 128 points gives error $< 10^{-9}$ at cost $\sim 128 + (11/4)b^2$. The QROM cost scales as the $1/3$ power of $1/\delta_f$ for quadratic interpolation, so the cost is remarkably insensitive to precision requirements.

### Non-cubic unit cells via Gramian arithmetic

For non-cubic cells, the squared norm $\|k_\nu\|^2$ requires computing:

$$g_{11} p_x^2 + g_{22} p_y^2 + g_{33} p_z^2 + 2g_{12} p_x p_y + 2g_{23} p_y p_z + 2g_{13} p_x p_z$$

The worst-case complexity is $(5/2)(n_x^2 + n_y^2 + n_z^2) + 2(n_x + n_y + n_z)^2 + 4b(n_x + n_y + n_z)$ Toffolis. But specific crystal structures have symmetries (e.g., $g_{11} = g_{22}$, or $g_{23} = g_{12} = 0$) that reduce this significantly — for diamond, the norm decomposes into three squares with no multiplications by real constants.

The paper allows **different numbers of bits** $(n_x, n_y, n_z)$ in each Miller direction, maintaining uniform reciprocal-space resolution regardless of cell geometry. This is a genuine improvement over Shokrian Zini et al. (2023), who used uniform grid sizes that produce non-uniform sampling for non-cubic cells.

### Kinetic energy via computed norm

Unlike [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]], which avoided computing $\|k_q\|^2$ entirely by decomposing the kinetic energy as a [[Kinetic Energy as Bit-Product LCU|bit-product LCU]], this paper *already computes* $\|k_q\|^2$ for the nonlocal pseudopotential. So it reuses this value for the kinetic energy block encoding via a simple inequality test. The cost of the kinetic energy is essentially absorbed into the pseudopotential computation.

---

## Key results

### Block encoding costs

For systems with $\eta = 100\text{–}500$ electrons, the block encoding cost $C_{\text{B.E.}}$ is $O(10^4)$ Toffolis:

| System | $\eta$ | Grid $(n_x, n_y, n_z)$ | $C_{\text{B.E.}}$ (Toffolis) |
|---|---|---|---|
| Pt $3\times 3$ slab | 270 | (6,6,7) | 32,931 |
| PtCO $3\times 3$ | 280 | (6,6,7) | 34,067 |
| Rh $3\times 3$ | 243 | (6,6,7) | 30,757 |
| C (diamond) $3\times 3\times 3$ | 216 | (6,6,6) | 23,576 |
| LNO-C2m $2\times 2\times 1$ | 92 | (5,5,5) | 18,569 |

The block encoding is 30–40× cheaper than Shokrian Zini et al. (2023), while simulating the *complete* pseudopotential (they omitted angular momentum projectors, introducing errors of hundreds of milliHartree per atom).

### Phase estimation costs

Total QPE Toffoli cost = $C_{\text{B.E.}} \times \lceil \lambda \pi / (2\varepsilon) \rceil$ with $\varepsilon = 1.6 \times 10^{-3}$ Hartree:

| System | $\lambda_{\text{nonloc}}$ | $\lambda_V$ | $\lambda_T$ | $\lambda_{\text{loc}}$ | QPE Toffolis |
|---|---|---|---|---|---|
| LNO-C2m (5,5,5) | 3,190,000 | 64,000 | 20,200 | 33,700 | $6.0 \times 10^{13}$ |
| Pt $3\times 3$ (6,6,7) | 34,200,000 | 787,000 | 115,000 | 164,000 | $1.1 \times 10^{15}$ |
| C diamond (6,6,6) | 7,530,000 | 541,000 | 110,000 | 222,000 | $1.9 \times 10^{14}$ |

$\lambda_{\text{nonloc}}$ dominates overwhelmingly — it's 50–100× larger than $\lambda_V$, which was the bottleneck in the non-pseudopotential case. This is the price of breaking the $l, i, j$ sum into separate terms and using box-level upper bounds.

### Comparison with second-quantized methods (LNO)

For the LiNiO₂ cathode (the benchmark from [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin et al. 2023]]):

| Method | Toffolis | Logical qubits |
|---|---|---|
| 2nd quant. DF (Rubin et al. 2023) | $\sim 5 \times 10^{12}$ | ~75,000 |
| 1st quant. PW + GTH (this paper) | $\sim 6 \times 10^{13}$ | ~1,400 |

First quantization costs ~12× more Toffolis but uses ~50× fewer qubits. The spacetime volume comparison favours first quantization.

### Error from omitting pseudopotential projectors

The paper demonstrates quantitatively that ignoring the off-diagonal angular momentum projectors (as done in Shokrian Zini et al. 2023) produces catastrophic errors:

| System | Energy error (Hartree) | Band gap error (eV) |
|---|---|---|
| AlN | 0.166 per unit cell | 3.28 (out of 3.60 total) |
| NiOF | 0.069 per unit cell | 0.73 (predicts gapless instead of 0.73 eV semiconductor) |
| LiMnO | 0.250 per unit cell | 0.50 |

These errors are *extensive* — they grow with system size. For AlN, the approximate pseudopotential gives a band gap of 0.32 eV instead of the correct 3.60 eV. This is not a small correction; it's a qualitatively different Hamiltonian.

---

## Comparison with prior work

| Feature | Su et al. 2021 | Shokrian Zini et al. 2023 | This paper |
|---|---|---|---|
| Pseudopotential | None (bare Coulomb) | GTH (diagonal only) | GTH (full, with nonlocal) |
| Non-cubic cells | No | Yes (uniform grid) | Yes (variable grid per direction) |
| Pseudopotential cost | N/A | $O(N)$ via QROM | $O(\log^2 N)$ via arithmetic |
| Angular momentum error | N/A | ~100s mHartree/atom | None (complete Hamiltonian) |
| Block encoding (LNO) | Not applicable | ~2.1M Toffolis | ~18.5K Toffolis |
| $\lambda$ for nonlocal PP | N/A | Not reported | $O(10^6\text{–}10^7)$ |

---

## Limits / caveats

1. **$\lambda_{\text{nonloc}}$ is enormous.** The nonlocal pseudopotential dominates $\lambda$ by 1–2 orders of magnitude over the electron-electron interaction. This comes from two sources: (a) breaking the $l, i, j$ sum into separate terms (summing absolute values instead of absolute value of the sum), and (b) using box-level upper bounds instead of tight $\nu$-dependent amplitudes. The authors note this as the main avenue for improvement.

2. **Large-core pseudopotentials only.** The resource estimates use large-core GTH-LDA pseudopotentials, which have fewer valence electrons but are less accurate than small-core variants. Using small-core pseudopotentials would roughly double the electron count, substantially increasing cost.

3. **No k-point sampling.** The approach uses supercells rather than k-point sampling for Brillouin zone integration. This means large simulation cells are needed to approach the thermodynamic limit, which drives up $\lambda_V$ and $\lambda_{\text{nonloc}}$.

4. **Accuracy of GTH vs PAW.** The GTH pseudopotential is accurate but not the most accurate available — PAW and ultrasoft pseudopotentials can do better for equation-of-state calculations. The HGH parameterisation used here captures scalar relativistic effects but has a known factor of 2–3 in root-mean-square EOS consistency error relative to PAW (per the Lejaeghere et al. 2016 benchmark).

5. **The CO/Pt puzzle remains unsolved.** The resource estimates for heterogeneous catalysis are $O(10^{14}\text{–}10^{15})$ Toffolis — enormous even by optimistic hardware projections. The paper frames this as motivation for quantum computing rather than a near-term application.

6. **First quantization vs second quantization tradeoff is system-dependent.** For LNO, first quantization wins on spacetime volume but loses on raw Toffoli count. For very large systems with many particles, first quantization's logarithmic dependence on $N$ should eventually win; for moderate sizes, the answer depends on the metric.

---

## Reusable ideas

1. [[QROM Interpolation for Negative Exponential]] — Convert $e^{-z}$ to $2^{-z/\ln 2}$, interpolate the fractional part via QROM polynomial, and bit-shift by the integer part. Achieves arbitrary precision with QROM cost scaling as $(1/\delta_f)^{1/3}$ for quadratic interpolation.

2. [[Shared Exponential Evaluation Across Pseudopotential Terms]] — Restructure the local and nonlocal pseudopotential so they share a single exponential evaluation. Setting $q = 0$ for the local part gives the correct argument, combining implementation of both terms through the same arithmetic pipeline.

3. [[Nested Box State Preparation with Overlapping Boxes]] — Prepare approximate amplitudes over momentum transfer $\nu$ using hierarchical boxes indexed by $\mu$. Each box provides an upper bound on the target function within it. The overlapping-box variant used here (where each $\nu$ belongs to all boxes $B_{\mu'}$ for $\mu' \geq \mu(\nu)$) boosts success probability and simplifies implementation.

4. [[Gramian-Based Momentum Norm for Non-Cubic Cells]] — Compute $\|k_\nu\|^2$ via the reciprocal lattice Gramian matrix with structure-specific optimisations that exploit symmetries ($g_{ij} = 0$, equal entries, etc.) to reduce the arithmetic cost well below the worst case.

5. [[Arithmetic vs QROM for Pseudopotential Evaluation]] — The architectural principle: when a function has analytical structure (as the GTH pseudopotential does), compute it with coherent arithmetic ($O(\text{polylog}\, N)$) rather than tabulating it via QROM ($O(N)$ or worse). This is the source of the ~100× improvement over Shokrian Zini et al.

---

## References within this paper

Key cited works and their role:

- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven (2019)]] — The original first-quantized plane-wave algorithm. This paper builds directly on its framework.
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] — The compiled, constant-factor version. This paper extends its block encoding to handle pseudopotentials and non-cubic cells.
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]] — The second-quantized materials benchmark (LNO). The first-vs-second quantization comparison in Section IX uses their resource estimates.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — Source of [[QROM (Quantum Read-Only Memory)|QROM]], [[Unary Iteration|unary iteration]], and [[Coherent Alias Sampling for PREPARE|coherent alias sampling]].
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry, Babbush et al. (2021)]] — THC qubitization for second-quantized chemistry; part of the comparative landscape.
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]] — Source of the exponential interpolation analysis and arithmetic costing used throughout.
- Shokrian Zini, Delgado, Arrazola et al. (2023) — The prior QROM-based pseudopotential approach. This paper corrects their omission of angular momentum projectors and improves block encoding cost by 30–100×.
- Goedecker, Teter, Hutter (1996); Hartwigsen, Goedecker, Hutter (1998) — Original GTH/HGH pseudopotential papers. Over 10,000 combined citations.

---

## Cross-links

### Paper notes
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — Direct predecessor; this paper extends its block encoding
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — Original first-quantized plane-wave framework
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — Second-quantized comparison point for LNO
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — QROM, unary iteration, alias sampling
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC qubitization; second-quantized comparison
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — Arithmetic costing, exponential interpolation
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — Earlier first-quantized approach (CI matrix)
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar, Babbush, Rubin 2024]]
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al 2023]]
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — the first-quantized real-space simulation paper this work descends from; Kivlichan et al. established the $\tilde{O}(\eta^2)$ query complexity for pairwise potentials that motivates the first-quantized plane-wave line
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]] — tackles the same basis-engineering problem from the molecular side; DG compresses the plane-wave dual primitive basis into block-local functions, while this paper makes full pseudopotential first quantisation practical via arithmetic-over-QROM

### Trick cards
- [[First-Quantized Plane-Wave Chemistry Encoding]] — The representation this paper builds on
- [[QROM (Quantum Read-Only Memory)]] — Used for nuclear positions and interpolation coefficients
- [[QROAM (Space-Time Tradeoff for QROM)]] — Advanced QROM variant referenced
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — The simulation framework
- [[Unary Iteration]] — Drives controlled swaps and QROM
- [[Coherent Alias Sampling for PREPARE]] — State preparation technique
- [[Kinetic Energy as Bit-Product LCU]] — The technique this paper *replaces* for the kinetic energy (since $\|k_q\|^2$ is already computed for the pseudopotential)
- [[Failed PREP Fallback to Alternative Hamiltonian Term]] — Still used for the local pseudopotential
- [[QROM Interpolation for Negative Exponential]] — New trick from this paper
- [[Shared Exponential Evaluation Across Pseudopotential Terms]] — New trick from this paper
- [[Nested Box State Preparation with Overlapping Boxes]] — New trick from this paper
- [[Gramian-Based Momentum Norm for Non-Cubic Cells]] — New trick from this paper
- [[Arithmetic vs QROM for Pseudopotential Evaluation]] — New trick from this paper
- [[Phase Gradient State for Controlled Rotations]] — Used for nuclear phase factors
