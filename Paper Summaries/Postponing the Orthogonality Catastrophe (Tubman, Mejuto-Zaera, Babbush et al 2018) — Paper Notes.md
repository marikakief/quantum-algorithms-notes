> **Source:** Norm M. Tubman, Carlos Mejuto-Zaera, Jeffrey M. Epstein, Diptarka Hait, Daniel S. Levine, William Huggins, Zhang Jiang, Jarrod R. McClean, Ryan Babbush, Martin Head-Gordon, K. Birgitta Whaley, *Postponing the orthogonality catastrophe: efficient state preparation for electronic structure simulations on quantum devices*, arXiv:1809.05523 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1809.05523)
> **Tags:** #state-preparation #phase-estimation #quantum-chemistry #NISQ #ASCI #initial-state #multi-determinant #overlap #FeMoco #Hubbard #electron-gas #DMFT

---

## The computational problem

For phase estimation (QPE) to succeed on a ground state energy estimation problem, the input state $|\psi_\text{in}\rangle$ must have non-negligible squared overlap with the true ground state $|\Psi_0\rangle$:

$$p_0 = |\langle \Psi_0 | \psi_\text{in} \rangle|^2$$

If $p_0$ is too small, the number of QPE repetitions needed to observe the ground state scales as $1/p_0$, potentially eliminating any quantum advantage. This is the **orthogonality catastrophe** as it applies to quantum simulation: overlaps between simple ansatz states and correlated ground states are expected to decay (possibly exponentially) as system size grows.

The paper addresses two questions:
1. Do efficiently preparable states have adequate ground-state overlap for chemically and physically interesting systems?
2. If single-determinant states are insufficient, how do we efficiently prepare richer multi-determinant reference states on a quantum computer?

## What the paper does

Uses the classical ASCI (Adaptive Sampling Configuration Interaction) method to estimate ground-state overlaps with various reference states across a broad range of systems — G1 molecules, transition metal complexes (Fe-porphyrin, FeMoco), homogeneous electron gas, Hubbard models, and DMFT impurity models. The headline finding: for most chemically interesting systems, HF or natural-orbital single Slater determinants have squared overlap $\geq 0.75$, and even strongly-correlated systems can be handled with tens to hundreds of determinants.

The paper also provides two explicit quantum circuits for preparing an $L$-term multi-determinant superposition $\sum_{\ell=1}^L \alpha_\ell |D_\ell\rangle$ with costs $O(L)$ and $O(nL)$ respectively, closing a gap in QPE resource accounting.

This is a practically important paper for anyone doing QPE resource estimates — it justifies assumptions about state preparation that earlier resource analyses (e.g. the FeMoco estimates in Reiher et al.) took on faith.

## The algorithm / construction

### ASCI ground-state approximation

ASCI (Adaptive Sampling CI) iteratively builds a compact classical approximation to the ground state by searching for the most important Slater determinants. The core ranking criterion uses the exact CI consistency relation:

$$C_i = \frac{\sum_{j \neq i} H_{ij} C_j}{E - H_{ii}}$$

where $H_{ij} = \langle D_i | H | D_j \rangle$. At each iteration, the right-hand side is evaluated using current-iteration $E$ and $\{C_j\}$, giving a first-order perturbative estimate of the coefficient magnitude for determinant $i$. Determinants are ranked by $|C_i|$ and the top candidates are added to the active space.

The **key observation** (Fig. 1 in the paper): the coefficient of the single most important determinant — and hence the single-determinant squared overlap — converges far more rapidly with the number of ASCI determinants than the total correlation energy does. For CN radical, the dominant determinant overlap reaches its converged value with $\sim 10^4$ determinants, while chemical accuracy in energy requires $> 10^6$. This makes ASCI-based overlap estimates reliable without full energy convergence.

### Multi-determinant state preparation: Method A (compressed register + QROM)

**Target state:** $|\psi_\text{in}\rangle = \sum_{\ell=1}^L \alpha_\ell |D_\ell\rangle$, where $|D_\ell\rangle$ are $n$-qubit occupation-number basis states and $L \ll 2^n$.

**Steps:**
1. Prepare a "compressed" register of $\lceil \log L \rceil$ qubits in the state $\sum_{\ell=1}^L \alpha_\ell |\ell\rangle$ (cost: $O(L)$ gates, via Shende-Bullock-Markov decomposition [ref 40]).
2. Apply an isometry $|\ell\rangle \mapsto |D_\ell\rangle$ mapping the compressed index register to the full $n$-qubit Fock state. This is implemented by either:
   - The **select-unitary** method of Childs et al. [arXiv:1711.10980], or
   - [[QROM (Quantum Read-Only Memory)]] (Babbush et al. [arXiv:1805.03662]).

Total cost: $O(L)$ gate depth from the compressed register preparation; the isometry adds terms polynomial in $n$ and $L$ (circuit-level costs depend on the specific implementation).

### Multi-determinant state preparation: Method B (single auxiliary qubit, sequential)

This is the paper's main algorithmic contribution. It avoids the compressed register entirely.

**Inductive construction:** Suppose after step $\ell$ we hold:

$$|\psi_\ell\rangle = \beta_\ell |D_\ell\rangle|1\rangle + \sum_{\ell'=1}^{\ell-1} \alpha_{\ell'} |D_{\ell'}\rangle|0\rangle$$

where $|\beta_\ell|^2 = 1 - \sum_{\ell'<\ell} |\alpha_{\ell'}|^2$ (normalization). Starting from $|\psi_1\rangle = |D_1\rangle|1\rangle$ (trivially prepared), one step of the induction proceeds:

1. **Rotation step.** Pick any qubit $k$ where $|D_\ell\rangle$ and $|D_{\ell+1}\rangle$ differ. Apply a controlled rotation on qubit $k$ (controlled on the auxiliary qubit being $|1\rangle$):

$$\beta_\ell |D_\ell\rangle|1\rangle \;\mapsto\; \left(\alpha_\ell |D_\ell\rangle + \beta_{\ell+1} X_k |D_\ell\rangle\right)|1\rangle$$

2. **Erase auxiliary for $|D_\ell\rangle$.** Uncompute the auxiliary qubit on the $|D_\ell\rangle$ branch using $O(n)$ controlled-NOT gates (controlled on the $n$-qubit state equaling $D_\ell$):

$$|D_\ell\rangle|1\rangle \;\mapsto\; |D_\ell\rangle|0\rangle$$

3. **Flip remaining differing qubits.** Apply NOT gates controlled by the auxiliary on all remaining positions where $D_\ell$ and $D_{\ell+1}$ differ. This produces:

$$\beta_\ell |D_\ell\rangle|1\rangle \;\mapsto\; \alpha_\ell |D_\ell\rangle|0\rangle + \beta_{\ell+1} |D_{\ell+1}\rangle|1\rangle$$

i.e. $|\psi_\ell\rangle \mapsto |\psi_{\ell+1}\rangle$.

**Total cost:** $O(nL)$ gates. Reducing the Hamming distance between neighbouring determinants $D_\ell, D_{\ell+1}$ reduces the constant.

## Key results

**Overlap results (empirical):**

| System | Reference state | Squared overlap | Notes |
|---|---|---|---|
| G1 molecule set (cc-pVTZ) | Hartree-Fock | $\geq 0.75$ (all cases) | Decreases with electron count |
| Fe-porphyrin (32e,29o) | HF/natural orbitals | 0.81–0.82 | Both spin states |
| Fe-porphyrin (44e,44o) | HF/natural orbitals | 0.73–0.82 | Varies by spin state |
| FeMoco (54e,54o) | HF/natural orbitals | 0.76–0.77 | Singlet |
| HEG 3D (14e, $r_s=0.5$) | Hartree-Fock | 0.97 | Weakly correlated |
| HEG 3D (14e, $r_s=2$) | Hartree-Fock | 0.75 | Borderline |
| HEG 3D (14e, $r_s=5$) | Hartree-Fock | $0.41 \pm 0.05$ | Needs multi-det |
| HEG 2D (10e, $r_s=5$) | Hartree-Fock | $0.22 \pm 0.02$ | Strongly correlated |
| N2 at $r=4$ Å | 20-determinant NO state | 0.96 | Multi-det rescues it |
| Cr2 at $r=2.5$ Å | 70-determinant NO state | 0.84 | Multi-det rescues it |
| 2D Hubbard, small $U/t$, low fill | Plane-wave HF | $\sim 1$ | Closed-shell structure |
| 2D Hubbard, half-fill, $U/t=8$ | Spatial HF | moderate | AF Mott phase |
| DMFT impurity, half-fill | Single det. | lower | Use multi-det |

**State preparation complexity:**

| Method | Gate cost | Extra qubits | Notes |
|---|---|---|---|
| Method A (compressed + QROM) | $O(L)$ (register prep) + isometry | $\lceil \log L \rceil$ compressed + ancilla | Isometry via [[QROM (Quantum Read-Only Memory)]] |
| Method B (sequential aux qubit) | $O(nL)$ | $1$ auxiliary | Hamming-ordering reduces constant |

**Key observation (ASCI convergence):**

$$\frac{\partial |\langle \Psi_0 | D_{\text{dominant}} \rangle|^2}{\partial (\text{# ASCI dets})} \;\gg\; \frac{\partial E_\text{corr}}{\partial (\text{# ASCI dets})}$$

Overlap estimates become reliable at much smaller ASCI expansion sizes than energy convergence requires. This is the practical justification for using ASCI to validate QPE initial state quality classically.

## Comparison with prior work

Prior resource estimates for QPE (Reiher/Wiebe/Svore/Wecker/Troyer 2017 on FeMoco; Babbush/Wiebe/McClean et al. 2018 on solid-state systems) all *assumed* that adequate initial states could be prepared, but did not verify this. Ward/Kassal/Aspuru-Guzik (2008) and McClean/Babbush/Love/Aspuru-Guzik (2014) made earlier attempts at overlap estimation, but used less capable classical methods.

The distinguishing feature here is ASCI's ability to reach systems with hundreds of orbitals (cc-pVTZ on large molecules, Hubbard models with $10^7$ determinants in active space) — far beyond what any prior method could certify.

For state preparation itself, Wecker/Hastings/Wiebe/Clark/Nayak/Troyer (2015) had proposed some mean-field ansatz approaches [ref 39]; the new sequential construction of Method B is more general and handles arbitrary multi-determinant expansions.

## Limits / caveats

- **Squared overlap is estimated, not exact.** ASCI gives a classical approximation; the true ground state is inaccessible classically for the interesting problem sizes. The authors validate this carefully (convergence plots, PT2 extrapolation), but it is still an estimate.
- **HEG at large $r_s$, DMFT below quarter-filling:** single-determinant overlaps fall below 0.5. Multi-determinant states help but require $\sim 100$ determinants to recover substantial support. The quantum state preparation cost then scales as $O(n \cdot 100)$, which is manageable but non-trivial.
- **Method B gate count is $O(nL)$, not $O(L \log n)$:** This is not optimal — each "erase" step costs $O(n)$ gates to check all $n$ orbital occupations. For $n = 100$ and $L = 100$ determinants, this is still $\sim 10^4$ gates, which is fine. For $n, L \sim 10^3$, it becomes $\sim 10^6$ gates just for state prep — possibly dominating the QPE circuit.
- **Hamming ordering is a heuristic:** The paper mentions ordering determinants to minimize Hamming distance as a cost reduction, but does not give a tight analysis of how much it helps or how to optimally order.
- **ASCI is not certifiably correct:** ASCI can get stuck in wrong phases (the Hubbard-model spin-density-wave vs. antiferromagnet example shows this explicitly). The overlap estimate depends on having converged to the correct ground state.
- **No T-gate or fault-tolerant resource analysis.** The paper does not compile the state preparation circuits to surface-code operations. For fault-tolerant QPE, the state prep circuits need T-gate counts too. Method B uses controlled-NOT operations whose fault-tolerant cost is straightforward but not reported here.
- **Orbital choice matters significantly.** Natural orbital rotations improve overlap for stretched molecules; the plane-wave basis is better than spatial basis for Hubbard at small $U/t$, and vice versa at large $U/t$ half-filling. Optimal basis choice is system-dependent and requires classical preprocessing.

## Reusable ideas

1. [[Sequential Multi-Determinant State Preparation]] — $O(nL)$-gate construction of $\sum_\ell \alpha_\ell |D_\ell\rangle$ using a single auxiliary qubit and inductive amplitude transfers. Completely general; works for any $L$ determinants, any $n$.
2. [[ASCI Overlap Estimation for QPE Initial State]] — use ASCI to classically certify that an efficiently-preparable state has adequate ground-state support, exploiting the fact that leading determinant coefficients converge much faster than correlation energy.
3. [[QROM (Quantum Read-Only Memory)]] — used in Method A for the index-to-determinant isometry; already documented in vault.
4. [[Hamming-Distance Ordering for Multi-Determinant Preparation]] — ordering the determinants in the sequential state prep protocol to minimise the number of qubit flips per step, reducing the gate count.

## References within this paper

| # | Reference | Notes |
|---|---|---|
| [3] | Aspuru-Guzik et al. (2005) | First QPE for chemistry proposal |
| [11] | Wecker, Bauer, Clark, Hastings, Troyer, Phys. Rev. A (2014) | Early resource estimate (FeMoco); assumed state prep was easy |
| [12] | Reiher, Wiebe, Svore, Wecker, Troyer, PNAS (2017) | FeMoco QPE resource estimate, also assumed state prep |
| [13] | [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al. (2018)]] | Source of QROM; fault-tolerant resource estimates for FeMoco-scale problems |
| [5] | [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush, Wiebe, McClean et al. (2018)]] | Solid-state QPE resource estimates; ignored state prep |
| [7] | [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes\|Kivlichan, McClean, Babbush et al. (2018)]] | NISQ state preparation via Givens rotations; single Slater determinants only |
| [20] | [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes\|Romero, Babbush et al. (2018)]] | UCC ansatz for VQE; alternative initial state approach |
| [25] | Tubman, Lee, Takeshita, Head-Gordon, Whaley, JCP (2016) | Original ASCI paper |
| [38] | Huron, Malrieu, Rancurel, JCP (1973) | Original CIPSI/perturbative CI ranking idea |
| [39] | Wecker, Hastings, Wiebe, Clark, Nayak, Troyer, PRA (2015) | Mean-field ansatz approaches to state prep for QPE |
| [40] | Shende, Bullock, Markov, IEEE Trans. CAD (2006) | $O(L)$ gates for arbitrary $L$-term state on $\log L$ qubits |
| [41] | Childs, Maslov, Nam, Ross, Su, arXiv:1711.10980 (2017) | Select-unitary method for index-to-state isometry |
| [70] | Mejuto-Zaera, Tubman, Whaley, arXiv:1711.04771 (2017) | ASCI-DMFT algorithm |

## Cross-links

### Paper notes
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — source of QROM used in Method A; also the paper that made QPE for FeMoco look concrete, motivating the need to close the state-prep gap
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — resource estimates for solid-state QPE that assumed state preparation was not a bottleneck
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — single-Slater-determinant preparation via Givens rotations; this paper extends to multi-determinant
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — VQE alternative to phase estimation; mentioned as likely insufficient for FeMoco
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — another paper whose resource estimates depended on availability of good initial states
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — the original FeMoCo resource estimate that assumed good initial-state overlap; this paper provides the first systematic verification of that assumption, finding HF overlap ~0.76–0.77 for FeMoCo
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — first-quantized plane-wave chemistry; state prep costs are different in that formalism
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — VQE measurement reduction; related context for hybrid algorithms
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — Overlap decay with system size is a shared concern: Tubman et al. study it for QPE initial states; QC-QMC faces the same issue for trial-walker overlaps in AFQMC
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — Addresses the same initial-state overlap problem from a different angle: compressed UCC provides a warm start for ADAPT-VQE on systems where HF has negligible overlap with the ground state
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC qubitization is one of the target algorithms for which this paper's initial-state overlap analysis is needed; Lee et al.'s $3.2 \times 10^{10}$ Toffoli FeMoCo estimate assumed good initial-state overlap which this paper begins to quantify
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — condensed-phase Trotter simulation; initial state quality (HF vs. multi-determinant) is equally important for periodic systems
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] — VQSE; alternative approach to improving overlap with correlated ground states without extra qubits, complementary to multi-determinant state prep

- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — extends the overlap analysis to Fe-S clusters where the orthogonality catastrophe is severe ($S^2 \sim 10^{-7}$ for FeMoCo); argues that when classical heuristics can achieve good overlap, they can also solve the energy problem
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings, Babbush, Rubin et al 2022]]
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]] — Takes a different approach to state preparation: MPS initial states prepared via improved unitary synthesis ($7\times$ better than LKS), rather than multi-determinant expansions. For FeMoCo, MPS at $\chi = 4000$ gives $\sim 0.9$ squared overlap — much higher than the product-state overlaps studied here.

### Trick cards
- [[Sequential Multi-Determinant State Preparation]] — Method B introduced in this paper
- [[ASCI Overlap Estimation for QPE Initial State]] — core classical technique of this paper
- [[Hamming-Distance Ordering for Multi-Determinant Preparation]] — cost reduction heuristic from this paper
- [[QROM (Quantum Read-Only Memory)]] — used in Method A
- [[Givens Rotation Slater Determinant Preparation]] — single-determinant case; what this paper extends
- [[Variational Quantum Eigensolver (VQE)]] — mentioned as likely insufficient for strongly correlated systems like FeMoco
- [[Active Space Restriction for VQE (CAS-UCC)]] — CASSCF/ASCI-SCF orbital optimisation mentioned
- [[Iterative Phase Estimation (Kitaev)]] — QPE is the target algorithm for which state preparation is being analysed
