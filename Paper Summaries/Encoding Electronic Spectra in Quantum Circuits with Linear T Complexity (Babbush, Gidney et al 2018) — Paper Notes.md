> **Source:** Ryan Babbush, Craig Gidney, Dominic W. Berry, Nathan Wiebe, Jarrod R. McClean, Alexandru Paler, Austin G. Fowler, *Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity*, arXiv:1805.03662, Phys. Rev. X **8**, 041015 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1805.03662) · [Journal](https://doi.org/10.1103/PhysRevX.8.041015)
> **Tags:** #quantum-simulation #quantum-chemistry #qubitization #LCU #fault-tolerant #T-complexity #Hubbard #electronic-structure #phase-estimation #QROM

---

## The computational problem

Given an electronic Hamiltonian in LCU form

$$H = \sum_{\ell=0}^{L-1} w_\ell H_\ell, \quad w_\ell \geq 0,\; H_\ell^2 = I, \quad \lambda \equiv \sum_\ell w_\ell$$

use quantum phase estimation to sample from the eigenbasis of $H$ with additive eigenvalue error at most $\varepsilon$. The problem is to construct the PREPARE and SELECT oracles needed for [[Qubitization (Quantum Walk for Spectral Encoding)]] as explicit, fault-tolerant quantum circuits with the minimum possible T-gate count.

Two Hamiltonians are targeted:
- **Electronic structure** in a second-quantized basis that diagonalizes the Coulomb operator (the "dual" / plane-wave-dual basis from [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush et al. 2018 (materials)]]):

$$H = \sum_{p,q} T(p-q) a^\dagger_p a_q + \sum_p U(p) n_p + \sum_{p\neq q} V(p-q) n_p n_q$$

with $\lambda = \sum_{p,q} |T(p-q)| + \sum_p |U(p)| + \sum_{p\neq q} |V(p-q)|$.

- **Fermi–Hubbard model** on a periodic square lattice with $N$ spin-orbitals:

$$H = -t \sum_{\langle p,q \rangle, \sigma} a^\dagger_{p,\sigma} a_{q,\sigma} + \frac{u}{2} \sum_p \sum_{\alpha \neq \beta} n_{p,\alpha} n_{p,\beta}$$

with $\lambda = 2Nt + Nu/2$.

---

## What the paper does

Constructs concrete, T-gate-optimized circuits for the PREPARE and SELECT oracles needed by qubitization, achieving T complexity $O(N + \log(1/\varepsilon))$ per oracle call and overall T complexity $O(N^3/\varepsilon + N^2 \log(1/\varepsilon)/\varepsilon)$ for full phase estimation on the electronic structure problem. This is asymptotically better than every prior approach and the paper also compiles to surface-code fault tolerance, showing that chemically interesting problems beyond classical reach require only about one million superconducting qubits and a few hours of runtime.

This is the paper that makes qubitization-based quantum chemistry practical, not just asymptotically optimal. The gap to prior work is not small: the gate count per oracle call goes from $O(N^4)$ in naive second-quantized LCU decompositions to $O(N)$ here, via three key circuit primitives introduced in this paper: [[Unary Iteration]], [[QROM (Quantum Read-Only Memory)]], and [[Coherent Alias Sampling for PREPARE]].

---

## The algorithm / construction

### 1. LCU decomposition

Write $H$ in LCU form (Eq. 5). For the electronic structure Hamiltonian in the dual basis this gives $L = O(N^2)$ terms — not $O(N^4)$ as in a naive second-quantized decomposition — because the basis diagonalizes the Coulomb operator, making the interaction terms diagonal in the computational basis. The 1-norm is $\lambda = O(N^2)$ for typical molecular problems at fixed electron density.

### 2. PREPARE oracle via coherent alias sampling

Define

$$\text{PREPARE}|0\rangle = |L\rangle \equiv \sum_\ell \sqrt{w_\ell / \lambda}\, |\ell\rangle \quad \text{(Eq. 6)}$$

(allowing extra ancilla "garbage" registers entangled with $|\ell\rangle$ as long as the required marginals hold).

**Implementation:** Use [[Coherent Alias Sampling for PREPARE]] — a quantum version of the classical Vose/alias sampling method:

1. Discretize probabilities $\tilde{\rho}_\ell = w_\ell/\lambda$ to $\mu$ bits (Eq. 36): $\mu = \lceil \log(\lambda N^2 / \varepsilon) \rceil$.
2. Classically precompute two tables per index $\ell$: `alt_ℓ` (an alternate index) and `keep_ℓ` (an integer in $[0, 2^\mu)$).
3. Coherently: prepare uniform superposition, use [[QROM (Quantum Read-Only Memory)]] to load `alt_ℓ` and `keep_ℓ`, generate uniform random $\sigma \in [0, 2^\mu)$, compare $\sigma < \text{keep}_\ell$ via a comparator, conditional-swap $\ell$ with `alt_ℓ`.

The resulting state (Eq. 34) has the correct marginals. The `subprepare` circuit handles the index register (spatial and spin orbital labels, plus a sign bit $\theta$); full cost is $6N + O(\mu + \log N)$ T gates and $2\mu + 3\log N + O(1)$ ancilla qubits (Fig. 15).

### 3. SELECT oracle via unary iteration

Define

$$\text{SELECT} = \sum_\ell |\ell\rangle\langle\ell| \otimes H_\ell \quad \text{(Eq. 7)}$$

The electronic-structure SELECT (`select_chem`) acts on index register $|\theta, U, V, p, \alpha, q, \beta\rangle$ (Eq. 44):
- $U=1, V=0$, $(p,\alpha) = (q,\beta)$: apply $Z_{p,\alpha}$
- $U=0, V=1$, $(p,\alpha) \neq (q,\beta)$: apply $Z_{p,\alpha} Z_{q,\beta}$
- $U=0, V=0$, $p < q$: apply $X_{p,\alpha} \overset{\scriptscriptstyle Z}{\cdots} X_{q,\alpha}$ (JW hopping)
- $U=0, V=0$, $p > q$: apply $Y_{q,\alpha} \overset{\scriptscriptstyle Z}{\cdots} Y_{p,\alpha}$ (JW hopping, conjugate)
- Global phase $(-1)^\theta$ via the sign bit.

The hopping terms require Jordan–Wigner strings, implemented via **selective Majorana operators** (Eq. 30): $a^\dagger_\ell - i a_\ell \mapsto Y_\ell Z_{\ell-1} \cdots Z_0$. These are implemented efficiently using [[Unary Iteration]] plus a ranged accumulator (XOR trick for JW strings), giving one pass over the system register per operator.

`select_chem` T-count: **$12N + 8\log N + O(1)$**, ancilla: $N + 3\log N + O(1)$ (Fig. 14).

The Hubbard SELECT is simpler (only hopping + on-site terms), achieving a smaller prefactor in the complexity.

### 4. Qubitized walk and phase estimation

Form the qubitized walk operator (Eq. 9):

$$W \equiv R_L \cdot \text{SELECT}, \quad R_L \equiv 2(|L\rangle\langle L| \otimes I) - I$$

where $R_L$ is the reflection about $|L\rangle$ implemented by PREPARE, reflection, PREPARE$^\dagger$.

The key property (Eqs. 14–15): on the two-dimensional subspace associated to eigenstate $|k\rangle$ of $H$ with eigenvalue $E_k$, $W$ acts as

$$W = \exp\!\left(i \arccos(E_k/\lambda)\, Y\right)$$

so the eigenphases of $W$ are $\pm \arccos(E_k/\lambda)$, and

$$E_k = \lambda \cos\!\left(\arg(\text{eigenphase}_k)\right) \quad \text{(Eq. 15)}$$

This is [[Qubitization (Quantum Walk for Spectral Encoding)]]: the spectrum of $H$ is exactly encoded (up to rotation synthesis) in the eigenphases of $W$.

Phase estimation is run on $W$ using the Heisenberg-limited protocol (Fig. 2, Eqs. 17–26): prepare the optimal resource state $\chi_m$ on $m$ ancilla qubits, apply controlled $\{W^n\}$ and $\{(W^\dagger)^n\}$, measure. The number of queries satisfies (Eq. 26):

$$2^m < \sqrt{2\pi}\, \lambda / \Delta E$$

and the total T complexity per phase estimation is (Eq. 27):

$$\approx \sqrt{2\pi}\, \frac{\lambda}{\Delta E}\, (S + 2P)$$

where $S$ and $P$ are the T-gate counts of SELECT and PREPARE respectively. Substituting $S = 12N + O(\log N)$ and $P = 6N + O(\mu + \log N)$:

$$T_\text{total} = 24\sqrt{2}\pi \frac{N\lambda}{\varepsilon} + O\!\left(\frac{\lambda}{\varepsilon} \log \frac{N}{\varepsilon}\right) \quad \text{(Theorem 1)}$$

---

## Key results

**Theorem 1 (Electronic structure):**

$$T\text{-gates} = 24\sqrt{2}\pi \frac{N\lambda}{\varepsilon} + O\!\left(\frac{\lambda}{\varepsilon} \log \frac{N}{\varepsilon}\right)$$

$$\text{Ancilla qubits} = \log\!\left(\frac{\lambda^3 N^5}{\varepsilon^3}\right) + O(1)$$

In standard parameterization ($\lambda = O(N^2)$): $T = O(N^3/\varepsilon + N^2 \log(1/\varepsilon)/\varepsilon)$.

**Theorem 2 (Hubbard model):**

$$T\text{-gates} = 10\sqrt{2}\pi \frac{N\lambda}{\varepsilon} + O\!\left(\frac{\lambda \log(N/\varepsilon)}{\varepsilon}\right)$$

$$\text{Ancilla qubits} = \log\!\left(\frac{\lambda N^3}{\varepsilon}\right) + O(1)$$

With $\lambda = 2Nt + Nu/2$: T scales as $O(N^2\lambda/\varepsilon)$.

**Per-oracle T-gate counts:**

| Subroutine | T gates | Qubits |
|---|---|---|
| SELECT (electronic structure) | $12N + 8\log N + O(1)$ | $N + 3\log N + O(1)$ |
| PREPARE / subprepare | $6N + O(\mu + \log N)$ | $2\mu + 3\log N + O(1)$ |
| Unary iteration (L items) | $4L - 4$ | $O(\log L)$ |
| QROM (L entries) | $4L - 4$ | $O(\log L)$ |
| **Combined oracle call** | $O(N + \log(1/\varepsilon))$ | $O(\log(\lambda N/\varepsilon))$ |

**Surface-code estimate:** Compiling to surface-code fault tolerance with per-gate error rate $10^{-3}$, the paper estimates that phase estimation on chemically interesting instances beyond classical tractability (e.g., FeMoco active space) requires ~1 million superconducting qubits and ~a few hours of runtime — a concrete, non-astronomical cost.

---

## Comparison with prior work

| Paper | Oracle cost | Total T / gates | λ scaling | Precision |
|---|---|---|---|---|
| Taylor series (2nd quant., database) [Babbush 2016] | $O(N^4)$ | $\tilde{O}(N^4 \lambda t)$ | $O(N^4)$ | $O(\log 1/\varepsilon)$ |
| Taylor series (on-the-fly) [Babbush 2016] | $O(N^4)$ | $\tilde{O}(N^5 t)$ | — | $O(\log 1/\varepsilon)$ |
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes\|CI matrix + Taylor series [Babbush 2018]]] | $\tilde{O}(N)$ | $\tilde{O}(\eta^2 N^3 t)$ | — | $O(\log 1/\varepsilon)$ |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Plane-wave dual + Trotter [Babbush 2018]]] | $O(N)$ | depth $O(N^{7/2} t^{3/2}/\sqrt{\varepsilon})$ | — | $O(1/\sqrt{\varepsilon})$ |
| **This work (qubitization + dual basis)** | $O(N + \log(1/\varepsilon))$ | $O(N^3/\varepsilon + N^2 \log(1/\varepsilon)/\varepsilon)$ | $O(N^2)$ | $O(1/\varepsilon)^*$ |
| Berry et al. (2019), QSVT | $O(N)$ | $O(N^3/\varepsilon)$ | $O(N^2)$ | $O(1/\varepsilon)$ |

\*Qubitization's $O(\lambda/\varepsilon)$ query scaling is Heisenberg-limited, not improvable. The comparison with Taylor series is subtle: Taylor achieves $O(\log(1/\varepsilon))$ Hamiltonian-simulation cost per fixed time window, but the phase-estimation wrapper for *spectroscopy* requires $O(1/\varepsilon)$ repetitions in both cases. Qubitization's advantage is that it directly encodes the spectrum without needing to simulate time evolution.

The key improvement over [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|the CI matrix approach]]: the CI approach achieves $\tilde{O}(\eta^2 N^3)$ which beats $O(N^3)$ when $\eta \ll N$, but the CI paper did not include compiled T-gate counts or a surface-code analysis. This paper provides end-to-end resource estimates.

---

## Limits / caveats

- **$O(\lambda/\varepsilon)$ is tight for spectroscopy but not simulation:** The $O(\lambda/\varepsilon)$ cost is Heisenberg-optimal for sampling in the eigenbasis (phase estimation). For simulating time evolution $e^{-iHt}$, qubitization gives $O(\lambda t)$ queries — no improvement over Taylor series in query complexity, but better constant factors in T gates.
- **$\lambda$ can be large:** For some basis choices, $\lambda = \Theta(N^2)$ which drives the overall $O(N^3/\varepsilon)$ scaling. In the first-quantized picture ([[CI Matrix Simulation (First-Quantized Encoding)|CI matrix]]), $\lambda$ can be smaller for systems with $\eta \ll N$. The dual basis is good for periodic systems but not necessarily optimal for all molecules.
- **No qubits savings vs. Taylor series:** The qubit count here is $O(\log(\lambda N/\varepsilon))$ — comparable to Taylor series. The CI matrix approach still wins on qubits for $\eta \ll N$.
- **Surface-code error rates assumed:** The million-qubit estimate assumes $10^{-3}$ per-gate error rate and specific surface-code overheads. Actual hardware thresholds will vary.
- **Rotation synthesis costs:** The circuits "exactly encode" the spectra *up to rotation synthesis errors*. The $O(\log(1/\varepsilon))$ T-gate cost for each Clifford+T rotation adds logarithmic terms to the total count, already captured in the theorem's second term.
- **Subsequent improvement:** The QSVT framework (Berry, Babbush et al. 2019) removes most of the $O(1)$ pre-factors and sub-leading terms. This paper is superseded for the electronic structure problem but remains the canonical reference for the circuit primitives (unary iteration, QROM, alias sampling).

---

## Reusable ideas

1. [[Qubitization (Quantum Walk for Spectral Encoding)]] — Use SELECT and PREPARE oracles to build a Szegedy-type walk $W$ whose eigenphases encode $H$'s eigenvalues as $\pm \arccos(E_k/\lambda)$. Perform phase estimation directly on $W$ to sample eigenstates. Cost: $O(\lambda/\varepsilon)$ oracle calls. Avoids explicit time evolution simulation and its error-propagation overheads.

2. [[Unary Iteration]] — Implement a controlled operation $\sum_\ell |\ell\rangle\langle\ell| \otimes U_\ell$ (indexed controlled unitaries over $L$ targets) with T-cost $4L - 4$ and $O(\log L)$ ancilla qubits. Key circuit primitive used throughout SELECT and QROM.

3. [[QROM (Quantum Read-Only Memory)]] — Coherently load classical lookup-table data: $|\ell\rangle|0\rangle \mapsto |\ell\rangle|d_\ell\rangle$ for all $\ell$ simultaneously. T-cost $4L - 4$, $O(\log L)$ ancilla qubits. Built on unary iteration with data-encoding CNOTs.

4. [[Coherent Alias Sampling for PREPARE]] — Prepare the LCU weight state $\sum_\ell \sqrt{w_\ell/\lambda}\,|\ell\rangle$ using a quantum version of the classical alias/Vose method. T-cost $O(L + \mu)$ where $\mu = O(\log(L/\varepsilon))$ bits. Additive (not multiplicative) dependence on precision bits — better than naive state preparation.

---

## References within this paper

| Ref | What it is | Vault note? |
|---|---|---|
| [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2017/2019)]] | "Qubitization" — the quantum walk / Szegedy-type framework this paper applies; this paper is the first detailed chemistry compilation of that framework | [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] |
| [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] | QSVT — subsequent work that subsumes this chemistry-specific construction into the general block-encoding / polynomial-transform framework | [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] |
| Babbush et al. (2018, PRX), 1711.07629 | Plane-wave dual basis and Trotter/LCU for materials — the basis encoding used here | [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] |
| Berry, Childs et al. (2015), FOCS | Truncated Taylor series LCU simulation — the prior LCU framework this supersedes for spectroscopy | No |
| Babbush et al. (2018), QST, 1506.01029 | CI matrix simulation — prior approach with better qubit count but larger pre-factors | [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] |
| Kivlichan, Wiebe, Babbush (2017) | Real-space simulation costs | [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] |
| Kivlichan, McClean et al. (2018) | Fermionic swap network — linear-depth Trotter steps | [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] |
| Babbush, Wiebe, McClean et al. (2018, Hubbard) | Low-depth simulation via FFFT | [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] |
| O'Malley, Babbush et al. (2016) | Experimental quantum chemistry | [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] |
| Wecker, Hastings, Troyer (2015) | Trotter analysis for molecular simulation | No |
| Aspuru-Guzik et al. (2005) | Original quantum chemistry simulation proposal | No |
| Vose (1991) | Classical alias sampling method | No |
| Poulin, Kitaev et al. (2018) | Quantum simulation by qubitization — companion theory paper | No |

---

## Cross-links

- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — the Trotter-based FeMoCo benchmark; this paper's qubitization approach represents the first major algorithmic paradigm shift beyond Trotter for chemistry resource estimation

### Paper notes
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — this paper uses the dual basis introduced there; the Hamiltonian encoding is taken directly from that paper's plane-wave dual construction
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — complementary approach by the same group; CI matrix is better on qubit count for $\eta \ll N$, qubitization is better on T-gate count and has compiled resource estimates
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — prior first-quantized approach; different basis (real space grid), same goal (reduce simulation cost)
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — Trotter-based approach; better for near-term (shallow circuits), worse asymptotically
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — hardware experiment; motivates the fault-tolerant direction this paper pursues
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — same group, same year; hybrid/variational side of the same research program
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — uses a first-quantized interaction-picture approach rather than second-quantized qubitization; achieves $\widetilde{O}(\eta^{8/3} N^{1/3} t)$ gate complexity (sublinear in $N$) with $O(\eta \log N)$ qubits; better gate complexity for molecular systems where $N \gg \eta$
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — compiles the first-quantized algorithms to fault-tolerant circuits, reusing the QROM and unary iteration primitives from this paper; shows first-quantized qubitization outperforms second-quantized qubitization (this paper) for realistic system sizes with $N \gtrsim 10^3$
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — direct extension to arbitrary molecular orbital bases via single factorization and sparse Coulomb; introduces [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] and [[Measurement-Based QROM Uncomputation|measurement-based uncomputation]] that improve on QROM from this paper; achieves $\widetilde{O}(N^{3/2}\lambda)$ Toffoli complexity for arbitrary bases
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — extends qubitization to arbitrary molecular orbital bases via THC factorization; reuses all four circuit primitives from this paper (unary iteration, QROM, coherent alias sampling, qubitization); achieves $\widetilde{O}(N\lambda_\zeta/\varepsilon)$ Toffoli complexity — the best known for second-quantized chemistry in arbitrary bases
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — applies QROM, unary iteration, and coherent alias sampling from this paper to combinatorial optimization oracles; extends QROM with variable-spacing variant for function evaluation; provides surface-code resource estimates for quantum optimization heuristics
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — this paper's QROM is used in the multi-determinant state preparation (Method A) of the orthogonality catastrophe paper; this paper's FeMoco resource estimate is one of the targets whose state-prep assumption the orthogonality catastrophe paper validates
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Rubin, Lee, Babbush 2021]]
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes|McClean, Babbush, Lin et al 2019]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al 2020]]
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes|Takeshita, Rubin, Babbush, McClean 2019]]
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al 2018]]
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — Uses the controlled Pauli string technique (Figure 9) for the Dirac operator block encoding in TDA; also uses [[Unary Iteration|unary iteration]] for Dicke state preparation
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]] — Qualtran's standard library is essentially a software implementation of this paper's constructions (unary iteration, QROM, coherent alias sampling, qubitization for Hubbard); reproduces FeMoCo resource estimates
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al 2022]]
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes|Babbush, Berry, Kothari, Somma, Wiebe 2023]]
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al 2023]]
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al 2022]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa, An, Sanders, Su, Babbush, Berry 2021]]
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al 2023]]
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry, Rubin, Babbush et al 2024]]
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes|Schmidhuber, O'Donnell, Kothari, Babbush 2024]]
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings, Babbush, Rubin et al 2022]]

### Trick cards
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the central framework; introduced in Low & Chuang, applied to chemistry here for the first time
- [[Unary Iteration]] — key circuit primitive: iterate over an index register with T-cost $4L-4$
- [[QROM (Quantum Read-Only Memory)]] — coherent table lookup; used in PREPARE and throughout
- [[Coherent Alias Sampling for PREPARE]] — prepare arbitrary LCU weight states efficiently
- [[Plane-Wave Dual Basis]] — the Hamiltonian encoding used here (from the materials paper)
- [[Truncated Taylor Series Simulation]] — the LCU time-evolution approach this paper partly supersedes for spectroscopy
- [[Iterative Phase Estimation (Kitaev)]] — wrapped by the qubitization framework; the measurement protocol
- [[Asymmetric Qubitization]] — variant of qubitization introduced in [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|Babbush, Berry, Neven (2019)]] that uses two different state oracles; the controlled Majorana circuit from Section 3B of *this* paper is the SELECT oracle used there
- [[QROAM (Space-Time Tradeoff for QROM)]] — improved version of QROM from [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry et al. 2019]] that trades ancilla for Toffoli gates
- [[Measurement-Based QROM Uncomputation]] — $M$-independent uncomputation of QROM outputs via X-basis measurement; introduced in [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry et al. 2019]]
- [[Directional Walk Control for Phase Doubling]] — the $U$-vs-$U^\dagger$ phase-doubling trick for eigenvalue estimation introduced in this paper was extended to Hamiltonian simulation by [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes|Berry, Motlagh, Pantaleoni, Wiebe (2024)]]

See [[FeMoCo Resource Estimation Timeline]] for how this estimate fits in the broader progression.
