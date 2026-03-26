> **Source:** Yuan Su, Dominic W. Berry, Nathan Wiebe, Nicholas Rubin, Ryan Babbush, *Fault-Tolerant Quantum Simulations of Chemistry in First Quantization*, arXiv:2105.12767, PRX Quantum **2**, 040332 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2105.12767) · [Journal](https://doi.org/10.1103/PRXQuantum.2.040332)
> **Tags:** #quantum-simulation #quantum-chemistry #first-quantized #qubitization #interaction-picture #fault-tolerant #plane-wave #resource-estimation #Toffoli-count

---

## The computational problem

Estimate eigenvalues of the Born-Oppenheimer electronic Hamiltonian

$$H = T + U + V$$

in a [[First-Quantized Plane-Wave Chemistry Encoding|first-quantized plane-wave basis]] to precision $\varepsilon$ (chemical accuracy $= 0.0016$ Hartree). Here $\eta$ is the number of electrons, $N$ the number of plane-wave basis functions, $\Omega$ the computational cell volume, and $L$ the number of nuclei. The state is stored in $\eta$ registers of $n_p = \lceil\log(N^{1/3}+1)\rceil$ qubits each, with total system qubits $n_s = 3\eta n_p$.

The goal: compile the two first-quantized algorithms from [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush et al. (2019)]] — [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] and [[Interaction Picture Simulation (Kinetic Frame)|interaction picture]] — to explicit, fault-tolerant circuits with optimised constant factors, then compare them to each other and to second-quantized methods.

---

## What the paper does

This is the first constant-factor resource estimate for *any* first-quantized chemistry algorithm, and the first concrete circuit compilation for any Dyson-series-based simulation. The paper introduces a suite of circuit-level optimisations that reduce the Toffoli complexity by roughly three orders of magnitude over naive implementations.

The bottom line: for realistic molecular and materials simulations, the first-quantized qubitization approach often requires **orders of magnitude less surface-code spacetime volume** than the best second-quantized Gaussian-orbital algorithms — even for small non-periodic molecules. And qubitization usually beats the interaction picture despite worse asymptotic scaling, because the constant factors are better for moderate $\eta$ and $N$.

This paper is where first-quantized quantum chemistry goes from "asymptotically nice" to "plausibly practical." It's the compiled, costed version of the theoretical framework in [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush et al. (2019)]].

---

## The algorithm / construction

### Qubitization approach

The Hamiltonian $H = T + U + V$ is expressed as a linear combination of unitaries with 1-norms:

$$\lambda_T = \frac{6\eta\pi^2}{\Omega^{2/3}}(2^{n_p-1}-1)^2 = O\!\left(\frac{\eta N^{2/3}}{\Omega^{2/3}}\right), \quad \lambda_U = O\!\left(\frac{\eta^2 N^{1/3}}{\Omega^{1/3}}\right), \quad \lambda_V = O\!\left(\frac{\eta^2 N^{1/3}}{\Omega^{1/3}}\right)$$

The qubitization walk operator $Q$ encodes eigenvalues as $e^{\pm i\arccos(E_k/\lambda)}$, requiring $O(\lambda/\varepsilon)$ applications.

**Key innovation #1 — Kinetic energy as bit products ([[Kinetic Energy as Bit-Product LCU]]):**

Instead of computing $\|p\|^2$ arithmetically ($\sim n_p^2$ Toffolis per electron), decompose it as

$$T = \frac{\pi^2}{\Omega^{2/3}} \sum_{j=1}^\eta \sum_{w \in \{x,y,z\}} \sum_{r,s=0}^{n_p-2} 2^{r+s} \sum_{b \in \{0,1\}} \left(\sum_{p \in G} (-1)^{b(p_{w,r} p_{w,s} \oplus 1)} |p\rangle\langle p|_j\right)$$

Each term in the LCU requires only a **single Toffoli** (to compute the bit product $p_{w,r} \cdot p_{w,s}$) plus a controlled-Z. The state preparation over $r, s$ uses cascading controlled Hadamards to create the weighted superposition $\sum_r 2^{r/2}|r\rangle$. This eliminates all multiplication circuits from the kinetic SELECT.

**Key innovation #2 — Failed PREP fallback ([[Failed PREP Fallback to Alternative Hamiltonian Term]]):**

The state preparation for $U+V$ (specifically the $\sum_\nu 1/\|\nu\| \,|\nu\rangle$ superposition) succeeds with probability $\approx 1/4$. Normally you'd use amplitude amplification ($3\times$ cost). Instead, when the preparation *fails*, apply SEL for $T$ in the failure branch. Since failure has probability $\approx 3/4$, this gives the correct relative weighting $3T/(4\lambda_T) + (U+V)/(4(\lambda_U + \lambda_V))$ when $\lambda_T = 3(\lambda_U + \lambda_V)$. An ancilla qubit rotation handles the general case. The effective $\lambda$ remains $\lambda_T + \lambda_U + \lambda_V$ without tripling any costs.

**PREPARE for $\nu$ weights:** Uses a hierarchy of nested boxes in momentum space, indexed by $\mu$, with an inequality test against $M(2^{\mu-2}/\|\nu\|)^2$ to approximate the $1/\|\nu\|$ weighting. Cost: $O(n_p^2 + n_M n_p)$ Toffolis.

**SELECT implementation:** Controlled swaps bring the relevant momentum registers $p_i, p_j$ into ancillae ($12\eta n_p$ Toffolis for the swap network using [[Unary Iteration]]). Arithmetic for $p + \nu$ and $q - \nu$ uses signed-integer addition ($24n_p$ Toffolis). Nuclear phase $-e^{-ik_\nu \cdot R_\ell}$ is applied via multiplication into a phase gradient register ($6n_p n_R$ Toffolis). Nuclear positions $R_\ell$ are loaded via [[QROM (Quantum Read-Only Memory)|QROM]] with cost $\lambda_\zeta + \mathrm{Er}(\lambda_\zeta)$.

**Total per-step Toffoli cost:** Dominated by the controlled swap network at $12\eta n_p$, plus arithmetic and state preparation terms (see Theorem 4, Eq. 125 in the paper). Phase estimation with $\pi\lambda/(2\varepsilon_{\mathrm{pha}})$ steps gives the overall complexity.

### Interaction picture approach

Set $A = T$ (kinetic, diagonal in momentum basis) and $B = U + V$ (potential). Work in the rotating frame of $T$ using the Dyson series.

**Key innovation #3 — Incremental kinetic energy register ([[Incremental Kinetic Energy Register]]):**

Rather than recomputing $\sum_j \|k_{p_j}\|^2$ at each step ($O(\eta n_p^2)$ Toffolis), maintain a running sum. When the block encoding of $U+V$ shifts momentum $p \to p+\nu$, update via:

$$\|p+\nu\|^2 - \|p\|^2 = 2p\cdot\nu + \|\nu\|^2$$

The dot product $p\cdot\nu$ costs $O(n_p^2)$ Toffolis (independent of $\eta$!), and $\|\nu\|^2$ is already computed from the PREPARE step. This reduces per-step kinetic energy cost from $O(\eta n_p^2)$ to $O(n_p^2 + n_\eta)$ — the dependence on $\eta$ drops to logarithmic.

**Key innovation #4 — Qubitized Dyson series ([[Qubitized Dyson Series for Phase Estimation]]):**

Instead of amplitude-amplifying a single Dyson step to get deterministic time evolution ($3\times$ cost), qubitize $\sin(H\tau)$ directly. The eigenvalues $\arcsin[\sin(E_j\tau)/e]$ are approximately $E_j\tau/e$ near zero. Setting $\tau = 1/\lambda_B$ gives effective evolution time $\tau_{\mathrm{eff}} = 1/(e\lambda_B)$, requiring $N_{\mathrm{steps}} = \pi e \lambda_B/(2\varepsilon_{\mathrm{pha}})$ steps. This is only $2/3$ the cost of the amplitude-amplified approach and $45\%$ of the method from Low & Wiebe.

**Dyson series state preparation:** The truncation order $K$ is typically 6–10. The $k$-superposition weights are $\sqrt{K!/k!}$ — square roots of integers — implemented exactly via inequality testing on an equal superposition of $\lceil K!e \rceil$ values. Time registers are prepared with controlled Hadamards and sorted with a quantum sorting network ($4n_t S_t(K)$ Toffolis).

**Higher-order time discretization (Appendix H):** Using midpoint-rule time discretization gives second-order error scaling, halving the required $n_t$ compared to prior work.

---

## Key results

**Theorem 4 (Qubitization):** Eigenvalue estimation to RMS error $\varepsilon$ with Toffoli count given by a detailed expression (Eq. 125) dominated by:

$$\frac{\pi\lambda}{2\varepsilon_{\mathrm{pha}}} \times (12\eta n_p + O(n_p^2 + n_p n_R + \lambda_\zeta))$$

with $\lambda = \max[\lambda_T' + \lambda_U^1 + \lambda_V^1,\; (\lambda_U^1 + \lambda_V^1/(1-1/\eta))/p_\nu^{\mathrm{amp}}]/P_{\mathrm{eq}}$.

**Theorem 5 (Interaction picture):** Toffoli count per step detailed in Eq. 173, multiplied by $N = \lceil\pi e(\lambda_U + \lambda_V/(1-1/\eta))/(2\varepsilon_{\mathrm{pha}} P_{\mathrm{eq}})\rceil$ steps.

**Asymptotic complexities** (at fixed $\varepsilon$, with $\Omega \propto \eta$):

| Algorithm | Toffoli complexity | Qubits |
|---|---|---|
| Qubitization | $\widetilde{O}(\eta^{4/3}N^{2/3}/\varepsilon + \eta^{8/3}N^{1/3}/\varepsilon)$ | $O(\eta \log N)$ |
| Interaction picture | $\widetilde{O}(\eta^{8/3}N^{1/3}/\varepsilon)$ | $O(\eta \log N)$ |

In terms of grid spacing $\Delta = (\Omega/N)^{1/3}$:

| Algorithm | Toffoli complexity |
|---|---|
| Qubitization | $\widetilde{O}(\eta^2/\Delta^2 + \eta^3/\Delta)$ |
| Interaction picture | $\widetilde{O}(\eta^3/\Delta)$ |

**Concrete resource estimates (Table VII):**

| System | $\eta$ | $N$ | Qubits | Toffolis |
|---|---|---|---|---|
| Ethylene carbonate (plane waves) | 46 | $2.6 \times 10^5$ | 2,021 | $1.7 \times 10^{11}$ |
| LiPF$_6$ (plane waves) | 72 | $2.6 \times 10^5$ | 2,556 | $5.1 \times 10^{11}$ |
| Ethylene carbonate (cc-pVTZ, Kim et al.) | 46 | 236 | 81,958 | $3.1 \times 10^{13}$ |
| LiPF$_6$ (cc-pVTZ, Kim et al.) | 72 | 244 | 84,721 | $3.3 \times 10^{13}$ |

The first-quantized approach with $N = 2.6 \times 10^5$ plane waves uses ~40× fewer qubits and ~100× fewer Toffolis than the second-quantized Gaussian approach with cc-pVTZ — despite using 1000× more basis functions. That's roughly three orders of magnitude less surface-code spacetime volume.

For condensed-phase materials at $r_s \approx 1{-}10\, a_0$, the qubitization algorithm requires $10^{10}{-}10^{12}$ Toffolis with $\sim 1000{-}5000$ logical qubits, translating to $\sim$few million physical qubits and $\sim$days of runtime under standard surface-code assumptions.

---

## Comparison with prior work

| Paper | Representation | Qubits | Toffoli / T complexity |
|---|---|---|---|
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al. 2018]] | 2nd-quant, dual basis, qubitization | $O(N)$ | $O(N^3/\varepsilon)$ T gates |
| Lee et al. 2021 (THC) | 2nd-quant, Gaussian, tensor hypercontraction | $O(N)$ | $\widetilde{O}(N^3/\varepsilon)$ |
| Kim et al. 2021 (von Burg/Lee) | 2nd-quant, Gaussian | $\sim 80{,}000$ | $\sim 10^{13}$ Toffolis (cc-pVTZ) |
| [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes\|Babbush et al. 2019]] | 1st-quant, plane-wave, interaction picture | $O(\eta\log N)$ | $\widetilde{O}(\eta^{8/3}N^{1/3}t)$ (asymptotic only) |
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes\|Babbush et al. 2018 (CI)]] | 1st-quant, CI matrix, Taylor series | $\widetilde{O}(\eta)$ | $\widetilde{O}(\eta^2 N^3 t)$ |
| **This work (qubitization)** | **1st-quant, plane-wave** | **$O(\eta\log N)$** | **$\widetilde{O}(\eta^{4/3}N^{2/3} + \eta^{8/3}N^{1/3})/\varepsilon$** |
| **This work (interaction picture)** | **1st-quant, plane-wave** | **$O(\eta\log N)$** | **$\widetilde{O}(\eta^{8/3}N^{1/3}/\varepsilon)$** |

The key finding: **qubitization is usually more practical than the interaction picture** for chemistry. The interaction picture wins only at small $\eta$ and extremely high resolution ($\Delta < 10^{-3} a_0$), which corresponds to a billion grid points per Bohr radius cubed — far beyond what's needed for electronic structure.

The ratio of interaction picture to qubitization Toffoli counts crosses unity only when $\eta \lesssim 20$ and $\Delta \lesssim 10^{-3} a_0$. For $\eta \geq 50$ or $\Delta \geq 10^{-2} a_0$, qubitization dominates. This is because at moderate $\eta$, the cost is dominated by the controlled swap network ($12\eta n_p$ per step for both algorithms), and the interaction picture adds Dyson-series overhead on top.

---

## Limits / caveats

**No pseudopotentials.** All resource estimates use all-electron calculations. Including pseudopotentials would reduce $\eta$ (by removing core electrons) and accelerate plane-wave convergence (by smoothing nuclear cusps). The paper flags this as the most important next step. Without pseudopotentials, the plane-wave basis converges slowly for heavy atoms.

**Cubic Bravais lattice only.** The reciprocal lattice is defined on a cubic grid (Eq. 6). Materials with non-cubic unit cells (hexagonal, monoclinic, etc.) would require non-orthogonal Bravais vectors, adding cost to the kinetic energy computation. This limits immediate applicability to a subset of crystal structures.

**State preparation in first quantization is under-developed.** Preparing arbitrary Slater determinants in first quantization costs $\widetilde{O}(\eta^2 M)$ for $M$ plane waves involved in the initial state, compared to $\widetilde{O}(\eta M)$ in second quantization. The paper notes this overhead but doesn't resolve it.

**Born-Oppenheimer only (in the numerics).** The algorithms trivially extend to non-Born-Oppenheimer dynamics (just add nuclear kinetic + interaction terms), but all concrete resource estimates are for the Born-Oppenheimer Hamiltonian.

**Comparison to Gaussian methods is approximate.** The claim that $N = 10^5$ plane waves matches cc-pVTZ accuracy is plausible (based on the $\sim 1000\times$ ratio from Appendix E of Babbush et al. 2018) but system-dependent. Pseudopotentials would improve this ratio significantly.

---

## Reusable ideas

1. [[Kinetic Energy as Bit-Product LCU]] — Decompose the kinetic energy $\|p\|^2$ as a sum of single-bit products $p_{w,r} \cdot p_{w,s}$, each requiring only one Toffoli. Eliminates all arithmetic from the kinetic SELECT oracle. Applicable whenever a diagonal operator involves squares of integer-valued registers.

2. [[Failed PREP Fallback to Alternative Hamiltonian Term]] — When PREPARE for one Hamiltonian term succeeds with probability $< 1$, use the failure branch to apply SELECT for a different term instead of paying $3\times$ for amplitude amplification. Applicable whenever $H$ is split into terms with separate PREPARE circuits and the $\lambda$ values allow re-balancing.

3. [[Incremental Kinetic Energy Register]] — Maintain a running register for $\sum_j \|k_{p_j}\|^2$ and update it incrementally ($O(n_p^2)$ per potential application) rather than recomputing from scratch ($O(\eta n_p^2)$). The $\eta$ dependence drops to logarithmic. Applicable to any interaction-picture simulation where $A$ is a function of the same registers modified by $B$.

4. [[Qubitized Dyson Series for Phase Estimation]] — Instead of amplitude-amplifying each Dyson-series step to get deterministic time evolution, qubitize $\sin(H\tau)$ directly and perform phase estimation on the resulting walk operator. Saves a factor of $3/2$ over amplitude amplification. Works for any Dyson-series-based simulation when the goal is eigenvalue estimation.

---

## References within this paper

| Reference | What it is | Vault note? |
|---|---|---|
| [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes\|Babbush et al. 2019]] [1] | The asymptotic paper this work compiles and optimises | ✓ |
| Low & Chuang 2019 [2] | [[Qubitization (Quantum Walk for Spectral Encoding)\|Qubitization]] framework | [[Qubitization (Quantum Walk for Spectral Encoding)]] |
| Low & Wiebe 2019 [3] | [[Interaction Picture Simulation (Kinetic Frame)\|Interaction picture]] technique | [[Interaction Picture Simulation (Kinetic Frame)]] |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al. 2018]] [21] | Qubitization circuits for 2nd-quantized chemistry (QROM, alias sampling, unary iteration) | ✓ |
| Lee et al. 2021 [28] | Tensor hypercontraction — best 2nd-quantized Gaussian algorithm | No |
| Kim et al. 2021 [86] | Resource estimates for battery chemistry using 2nd-quantized methods | No |
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes\|Babbush et al. 2018 (CI)]] [63/22] | CI matrix approach, Berry, Kieferová et al. eigenstates paper | ✓ |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. 2018 (materials)]] [30] | Plane-wave dual-basis + FFFT for 2nd-quantized materials simulation | ✓ |
| Kivlichan, Gidney et al. 2020 [25] | Improved Trotter for condensed-phase plane waves | No |
| Su, Huang, Campbell 2021 [29] | Nearly tight Trotter bounds for interacting electrons | No |
| Kassal et al. 2008 [41] | Original proposal for first-quantized chemistry simulation | No |
| Berry, Childs et al. 2015 [7] | [[Truncated Taylor Series Simulation]] | [[Truncated Taylor Series Simulation]] |

---

## Cross-links

- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — original second-quantized FeMoCo benchmark; this paper provides a first-quantized alternative for the same class of problems

### Paper notes
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — the asymptotic predecessor; this paper compiles and optimises those algorithms, adding $\sim 1000\times$ circuit-level improvements and the first constant-factor analysis
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — introduces the circuit primitives ([[Unary Iteration]], [[QROM (Quantum Read-Only Memory)|QROM]], [[Coherent Alias Sampling for PREPARE]]) reused heavily here; the second-quantized qubitization approach that this paper's first-quantized version outperforms
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — earlier first-quantized approach with $\widetilde{O}(\eta^2 N^3)$; this paper renders CI obsolete for large $N$
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — second-quantized plane-wave simulation; this paper's first-quantized approach is dramatically cheaper for the same materials at large $N$
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — first-quantized real-space simulation; Appendix K of this paper introduces new real-space algorithms matching the plane-wave scaling
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — Rubin is a co-author on both; operator compression techniques are complementary (second-quantized compression vs. first-quantized encoding)
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — QC-QMC provides an alternative (NISQ-compatible) path to the same chemical accuracy targets
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — the best second-quantized competitor; THC qubitization achieves $\widetilde{O}(N\lambda_\zeta/\varepsilon)$ in arbitrary bases; for small molecules (FeMoCo-scale) the THC approach often wins, but first-quantized qubitization should dominate for $N \gg \eta$
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al 2020]]
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes|McClean, Babbush, Lin et al 2019]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al 2020]]
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al 2018]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush 2019]]
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — Questions whether the problems these algorithms target (ground-state chemistry) actually exhibit exponential quantum advantage; the excellent asymptotics here are real, but the classical competition (DMRG, coupled cluster, PEPS) may also be polynomial
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — Uses the algorithms compiled here (including Appendix K grid basis) to show quantum advantage over classical mean-field dynamics; tightens Trotter bounds for the dynamics use case; introduces first-quantized classical shadows and second-to-first quantization state conversion
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]] — Uses the first-quantised plane-wave block encoding from this paper to construct force operators; shows $\lambda_F$ has the same asymptotic scaling as $\lambda_H$ in plane waves, and force operators are one-body under the FFFT/QFT
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — The second-quantized Bloch orbital alternative for periodic materials; uses Gaussian basis instead of plane waves. This paper (first-quantized) has fundamentally better basis-size scaling ($\eta^{4/3} N^{2/3}$ vs $N_k N$), but Bloch orbitals need far fewer basis functions for localized phenomena
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — Direct successor: extends this paper's block encoding to include GTH pseudopotentials and non-cubic unit cells; arithmetic-based evaluation replaces bare Coulomb nuclear potential with full screened potential; compares first-vs-second quantisation for LNO cathode
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — Extends this paper's block encoding to non-Born-Oppenheimer dynamics with a quantum projectile; first constant-factor resource estimate for a quantum dynamics simulation; also introduces hybrid QROM+Newton-Raphson for inverse square root in product formulas
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar, Babbush, Rubin 2024]]
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings, Babbush, Rubin et al 2022]]

### Trick cards
- [[First-Quantized Plane-Wave Chemistry Encoding]] — the representation used throughout
- [[Interaction Picture Simulation (Kinetic Frame)]] — the framework for the second algorithm
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the framework for the first algorithm
- [[Kinetic Energy as Bit-Product LCU]] — new: eliminates arithmetic from kinetic SELECT
- [[Failed PREP Fallback to Alternative Hamiltonian Term]] — new: avoids amplitude amplification
- [[Incremental Kinetic Energy Register]] — new: cheap kinetic energy updates in the interaction picture
- [[Qubitized Dyson Series for Phase Estimation]] — new: qubitize instead of amplitude-amplify the Dyson step
- [[Unary Iteration]] — used for controlled swap of momentum registers
- [[QROM (Quantum Read-Only Memory)]] — used for nuclear position lookup
- [[Coherent Alias Sampling for PREPARE]] — not directly used (this paper uses nested-box preparation instead), but the related technique from [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]]
- [[LCU Sign-Parity Cancellation for Grid Boundaries]] — used for handling momentum shifts outside the grid
- [[Truncated Taylor Series Simulation]] — the LCU formalism underlying both algorithms
- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]] — Builds on the first-quantized framework from this paper; reduces the Coulomb bottleneck from $O(\eta^2)$ to $\widetilde{O}(\eta)$ using quantum FMM
