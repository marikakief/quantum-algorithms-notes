> **Source:** Dominic W. Berry, Craig Gidney, Mario Motta, Jarrod R. McClean, Ryan Babbush, *Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization*, arXiv:1902.02134, Quantum **3**, 208 (2019)
> **Links:** [arXiv](https://arxiv.org/abs/1902.02134) · [Journal](https://doi.org/10.22331/q-2019-12-02-208)
> **Tags:** #quantum-simulation #quantum-chemistry #qubitization #LCU #fault-tolerant #T-complexity #FeMoCo #low-rank #sparsity #QROAM #block-encoding #molecular-orbital

---

## The computational problem

Given the electronic structure Hamiltonian in an arbitrary second-quantized basis of $N$ spin-orbitals:

$$H = \sum_{\sigma} \sum_{p,q} T_{pq}\, a^\dagger_{p,\sigma} a_{q,\sigma} + \sum_{\alpha,\beta} \sum_{p,q,r,s} V_{pqrs}\, a^\dagger_{p,\alpha} a_{q,\alpha}\, a^\dagger_{r,\beta} a_{s,\beta}$$

where $T_{pq}$ are one-body integrals and $V_{pqrs}$ are two-electron integrals (chemist notation, with eightfold symmetry), construct a [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] quantum walk operator whose eigenphases encode the eigenvalues of $H$. Use quantum phase estimation to estimate eigenvalues to additive error $\Delta E$. The cost model counts Toffoli gates, since these dominate surface-code spacetime volume.

The target system is the FeMo cofactor of nitrogenase (FeMoCo, Fe$_7$MoS$_9$C), a catalytic iron-sulfur cluster whose electronic structure is beyond classical exact methods. Two active spaces are considered: the 108 spin-orbital space of Reiher, Wiebe, Svore, Wecker, and Troyer (RWSWT, 2017) and the 152 spin-orbital space of Li, Li, Dattani, Umrigar, and Chan (LLDUC, 2019).

---

## What the paper does

Extends [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based quantum chemistry from the structured plane-wave dual basis (where [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]] achieved $O(N)$ per-oracle T cost) to **arbitrary molecular orbital bases** — the Gaussian-type bases that most of chemistry actually uses. The plane-wave dual basis has $\Theta(N^2)$ terms and diagonal Coulomb structure; molecular orbital bases have $O(N^4)$ terms and no such structure. This paper shows how to recover efficient qubitization despite this, using two independent strategies:

1. **Single factorization (low rank):** Diagonalize the $N^2/4 \times N^2/4$ Coulomb supermatrix $W$ into $L = O(N)$ eigenvectors, giving an LCU with $O(N^3)$ distinct coefficients instead of $O(N^4)$. State preparation via QROAM (an improved [[QROM (Quantum Read-Only Memory)|QROM]] with space-time tradeoffs) achieves $\widetilde{O}(N^{3/2}\lambda)$ T complexity.

2. **Sparse Coulomb:** Threshold-truncate $V_{pqrs}$ to zero out near-zero entries, then use a [[Sparse State Preparation for Qubitized Chemistry|sparse state preparation]] where QROAM cost scales with the number of nonzero entries $L_V^{(c)}$ rather than the tensor dimension $N^4/16$.

The best concrete result: **$8.4 \times 10^{10}$ Toffolis** for FeMoCo at chemical accuracy using the LLDUC orbitals and sparse Coulomb approach — about **$700\times$ less surface-code spacetime** than the original Reiher et al. estimate ($\sim 10^{14}$ T gates), despite using a larger active space.

My assessment: this paper is the bridge between the elegant but impractical plane-wave qubitization of [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]] and the later THC-based approach of [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. 2021]]. It's the first paper to make qubitization work for molecular orbitals at competitive constant factors, and the QROAM and measurement-based uncomputation techniques introduced here became standard tools in every subsequent fault-tolerant chemistry paper. The single factorization idea draws directly from [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta et al. 2018]] but applies it in the LCU setting rather than Trotter.

---

## The algorithm / construction

### Step 1: Single factorization of the Coulomb operator

Reshape $V_{pqrs}$ into an $N^2/4 \times N^2/4$ matrix $W$ with composite indices $pq$ and $rs$ (chemist notation ordering is essential — physicist notation destroys the required structure). $W$ is real symmetric and positive semideﬁnite. Diagonalize:

$$W = \sum_{\ell=1}^{L} \omega_\ell\, g^{(\ell)} (g^{(\ell)})^T$$

where $g^{(\ell)}_{pq}$ are the eigenvectors and $\omega_\ell \geq 0$ the eigenvalues. The rank $L = O(N)$ — this is the density fitting / Cholesky decomposition structure long exploited in classical electronic structure. The two-electron operator factors as:

$$V = \sum_\ell \omega_\ell \left(\sum_{p,q,\sigma} g^{(\ell)}_{pq}\, a^\dagger_{p,\sigma} a_{q,\sigma}\right)^2$$

This reduces the distinct coefficient count from $O(N^4)$ to $O(N^2 L) = O(N^3)$.

The truncation rank $L$ is chosen by monitoring convergence of classically computed CISD and MP2 correlation energies under truncation. For both RWSWT and LLDUC FeMoCo active spaces, $L = 200$ suffices for chemical accuracy (a coincidence — the two integral sets have different properties).

### Step 2: LCU decomposition via Jordan-Wigner

Map $H$ to qubits using the Jordan-Wigner transformation. The one-body terms give $X_p \vec{Z} X_q + Y_p \vec{Z} Y_q$ hopping strings and $Z_p$ number operators. The factorized two-body terms give products of these operators. The LCU 1-norm decomposes as $\lambda = \lambda_T + \lambda_W$ where:

$$\lambda_T = 2\sum_{p,q} |T_{pq}|, \quad \lambda_W = 4\sum_\ell \omega_\ell \left(\sum_{p,q} |g^{(\ell)}_{pq}|\right)^2$$

The factorization inflates $\lambda$ slightly: $\lambda_V \leq \lambda_W$ (by triangle inequality), but asymptotically the scaling is the same. For RWSWT: $\lambda = 36{,}042$ a.u. For LLDUC: $\lambda = 24{,}192$ a.u.

### Step 3: State preparation (PREPARE) via QROAM

The state to prepare has the form (schematically):

$$|\psi\rangle = |0\rangle\sqrt{|T_{pq}|/\lambda}\,|p,q,\sigma\rangle + \sum_\ell \sqrt{\omega_\ell/\lambda}\,|\ell\rangle\sqrt{|g^{(\ell)}_{pq} g^{(\ell)}_{rs}|}\,|p,q,\alpha\rangle|r,s,\beta\rangle$$

There are $(L+1)(N^2/8 + N/4) = O(N^3)$ unique coefficients (exploiting the symmetry $g^{(\ell)}_{pq} = g^{(\ell)}_{qp}$ to prepare only the upper triangle, then use controlled swaps to symmetrize).

**QROAM** ([[QROAM (Space-Time Tradeoff for QROM)|QROAM]]) is the main technical contribution for reducing the PREPARE cost. It improves over the [[QROM (Quantum Read-Only Memory)|QROM]] of [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]] by trading ancilla qubits for Toffoli gates. Given a table of $d$ entries with output size $M$ bits:

- **Clean ancillae:** Compute costs $\lceil d/k \rceil + M(k-1)$ Toffolis for parameter $k$; optimal $k \approx \sqrt{d/M}$ gives $\sim 2\sqrt{dM}$ Toffolis.
- **Dirty ancillae:** Compute costs $2\lceil d/k \rceil + 4M(k-1)$; optimal with $k$ bounded by available dirty qubits (often the system register provides $N$ dirty qubits).
- **Measurement-based uncomputation:** Uncomputing costs only $\lceil d/k \rceil + k$ Toffolis (clean) or $2\lceil d/k \rceil + 4k$ (dirty) — **independent of $M$**. This is the biggest single improvement: X-basis measurements on output qubits, followed by classically conditioned phase fixups, avoid the $M$-dependent uncomputation cost entirely.

The combined compute+uncompute cost with clean ancillae at optimal $k$: $\sim 2\sqrt{d}(\sqrt{M}+1)$.

The state preparation merges the $\ell$-register preparation with the $(p,q)$-register preparation using the contiguous index $s = \ell(N^2/8 + N/4) + p(p+1)/2 + q$, saving a factor of $L$ in the inequality-test costs.

### Step 4: Controlled unitaries (SELECT)

SELECT applies the Jordan-Wigner hopping operators $X_p \vec{Z} X_q$ or $Y_p \vec{Z} Y_q$ (distinguished by $p < q$ vs $p > q$), or the identity/number operators for $p = q$, controlled on the index registers. The circuit (Figure 1 of the paper) uses [[Unary Iteration]] over the system register, inequality tests between $p$ and $q$, and an $S$ gate to fix phases. Cost: $O(N)$ Toffolis per step — subdominant to state preparation.

Two SELECT operations are needed per walk step (one for each electron pair register), for a total of $4N + 4\lceil\log N\rceil$ Toffolis.

### Step 5: Phase estimation

Run phase estimation on the qubitized walk operator with $m = \lceil\log(\sqrt{2}\pi\lambda / 2\Delta E)\rceil$ steps. The allowable per-step error is $\varepsilon = \sqrt{2}\Delta E / 4\lambda$.

### Step 6: Sparse Coulomb variant

Instead of factorizing, threshold-truncate the raw $V_{pqrs}$:

$$\tilde{V}^{(c)}_{pqrs} = \begin{cases} V_{pqrs} & |V_{pqrs}| \geq c \\ 0 & |V_{pqrs}| < c \end{cases}$$

Choose $c$ so CISD/MP2 correlation energies remain within chemical accuracy. The number of nonzero entries $L_V^{(c)}$ is system-dependent but empirically much smaller than $N^4/16$.

Use a modified [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] where the QROAM iterates over the $L_V^{(c)}/8$ unique nonzero entries (after exploiting eightfold symmetry), outputting both the index $(p,q,r,s)$ and the alternate/keep values. Three controlled swaps then generate the remaining symmetry copies. The key advantage: QROAM cost depends on the number of nonzero entries, not the full $N^4$ tensor dimension.

For LLDUC orbitals: $c = 10^{-4}$ a.u. gives $L_V^{(c)} = 1{,}291{,}648$ nonzero entries (179,498 unique after symmetry), and $\lambda_V = 4{,}168$ a.u. — much smaller than the factorized $\lambda_W = 20{,}746$ a.u.

---

## Key results

| Variant | Active space | Toffolis | Logical qubits | Notes |
|---|---|---|---|---|
| Single factorization, dirty ancilla | RWSWT (108) | $2.1 \times 10^{13}$ | 378 | Minimal qubit count |
| Single factorization, many ancilla | RWSWT (108) | $1.2 \times 10^{12}$ | 3,024 | $\sim 18\times$ fewer Toffolis |
| Single factorization, dirty ancilla | LLDUC (152) | $2.0 \times 10^{13}$ | 437 | |
| Single factorization, many ancilla | LLDUC (152) | $9.8 \times 10^{11}$ | 3,143 | |
| Sparse Coulomb | RWSWT (108) | $2.3 \times 10^{11}$ | 5,103 | Best for RWSWT |
| **Sparse Coulomb** | **LLDUC (152)** | $\mathbf{8.4 \times 10^{10}}$ | **2,903** | **Best overall** |
| Reiher et al. 2017 (prior) | RWSWT (108) | $\sim 5 \times 10^{13}$ | 111 | Trotter-based |

Leading-order T complexities:
- **Single factorization, dirty ancillae:** $\widetilde{O}(\lambda N M L / \Delta E)$
- **Single factorization, many ancillae:** $\widetilde{O}(\lambda N \sqrt{LM} / \Delta E)$
- **Sparse Coulomb:** $\widetilde{O}(\lambda L_V^{(c)} / \Delta E)$ (practical), $O(\lambda N^4 / \Delta E)$ (worst-case asymptotic)

Surface-code spacetime: the best variant requires $\sim 3$ megaqubitweeks (at $10^{-3}$ physical error rate and $\sim 24$ qubitseconds per Toffoli distillation). This is $\sim 700\times$ better than Reiher et al.'s $\sim 2$ gigaqubitweeks.

---

## Comparison with prior work

| Method | T / Toffoli complexity (asymptotic) | FeMoCo Toffolis | Basis type |
|---|---|---|---|
| Reiher et al. 2017 (Trotter) | Large (not tightly bounded) | $\sim 5 \times 10^{13}$ | Molecular orbital |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al. 2018]] | $O(N^3/\varepsilon)$ per PE | $\sim 10^8$ | Plane-wave dual only |
| [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes\|Kivlichan, Gidney et al. 2020]] | Trotter-based | N/A for FeMoCo | Plane-wave dual |
| Babbush et al. 2016 (Taylor) | $\widetilde{O}(\lambda^2 t)$ | N/A | Arbitrary |
| Campbell 2019 (random compiler) | $\widetilde{O}(\lambda^2)$ | N/A | Arbitrary |
| **This paper (single factorization)** | $\widetilde{O}(N^{3/2}\lambda)$ | $9.8 \times 10^{11}$ | **Arbitrary** |
| **This paper (sparse)** | System-dependent | $8.4 \times 10^{10}$ | **Arbitrary** |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes\|Lee, Berry et al. 2021]] (later) | $\widetilde{O}(N\lambda_\zeta/\varepsilon)$ | $5.3 \times 10^9$ | Arbitrary |

The $\widetilde{O}(N^{3/2}\lambda)$ scaling beats all prior methods when $\lambda = \Omega(N^{3/2})$, which holds empirically: numerics on hydrogen chains show $\lambda_V = O(N^{2.2})$ towards the thermodynamic limit and $\lambda_V = O(N^{2.7})$ towards the continuum limit.

---

## Limits / caveats

- **Sparse approach is not asymptotically better.** The number of nonzero Coulomb entries after thresholding is still $O(N^4)$ in the worst case. The improvement is purely from constant-factor sparsity in practical molecular systems. For extended systems with long-range interactions, the sparsity benefit diminishes.

- **$\lambda_W > \lambda_V$:** The factorization inflates the LCU 1-norm (by the triangle inequality). Numerically, $\lambda_W / \lambda_V$ scales as roughly $O(N^{0.3})$. This tax is more than offset by the $\sqrt{N}$ reduction in QROAM cost from having $O(N^3)$ instead of $O(N^4)$ coefficients, but it's not free.

- **$L = O(N)$ is empirical.** The $O(N)$ rank of the Coulomb supermatrix is numerically well-established and physically motivated (pairwise Coulomb interactions), but not rigorously proven for all molecular systems. The same caveat applies to the [[Double Factorization of Two-Electron Integrals|double factorization]] of [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta et al. 2018]].

- **Spacetime volume is still large.** Even $8.4 \times 10^{10}$ Toffolis × 24 qubitseconds = $\sim 3$ megaqubitweeks. Running in a day needs $\sim 23$ million physical qubits.

- **Second factorization not exploited.** The paper notes that each $g^{(\ell)}_{pq}$ matrix has only $O(\log N)$ significant eigenvalues (the same observation that drives [[Double Factorization of Two-Electron Integrals|double factorization]] for Trotter), but does not exploit it for the LCU approach. Later work by [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. 2021]] achieves something similar through THC.

- **Chemist vs. physicist notation matters.** The Coulomb supermatrix has low rank only in chemist ordering $V_{pqrs} = \langle pq|rs \rangle$. Physicist ordering or spin-orbital-level symmetry reduction destroys the structure. This is a subtle but important implementation detail.

---

## Reusable ideas

1. [[QROAM (Space-Time Tradeoff for QROM)]] — Space-time tradeoff for [[QROM (Quantum Read-Only Memory)|QROM]] using clean or dirty ancillae and a swapping network. Reduces Toffoli cost from $O(d)$ to $O(\sqrt{dM})$ for $d$ table entries with $M$-bit outputs. Became the standard QROM implementation in all subsequent fault-tolerant chemistry papers.

2. [[Measurement-Based QROM Uncomputation]] — X-basis measurements on QROM output qubits, followed by classically conditioned phase fixups, uncompute a table lookup at cost $O(\sqrt{d})$ Toffolis independent of output size $M$. This removes the $M$-dependence entirely from the uncomputation step.

3. [[Single Factorization for Qubitized Chemistry]] — Eigendecomposition of the Coulomb supermatrix (chemist ordering) reduces the LCU coefficient count from $O(N^4)$ to $O(N^3)$, enabling $\widetilde{O}(N^{3/2}\lambda)$ qubitization in arbitrary molecular orbital bases.

4. [[Sparse State Preparation for Qubitized Chemistry]] — Modified [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] that iterates over nonzero entries rather than the full index space. The QROAM outputs both the coefficient index and the alias/keep values, with cost proportional to the number of nonzero entries. Controlled swaps then generate symmetry copies.

---

## References within this paper

Key cited works with vault links:

- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]] — The predecessor paper. All the core circuit machinery (qubitization for chemistry, [[Unary Iteration]], [[QROM (Quantum Read-Only Memory)|QROM]], [[Coherent Alias Sampling for PREPARE|alias sampling]]) originates here. This paper extends those techniques to arbitrary bases.
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al. 2018]] — Introduces the single and [[Double Factorization of Two-Electron Integrals|double factorization]] of Coulomb integrals for quantum simulation. This paper takes the single factorization and applies it in the LCU/qubitization setting instead of Trotter.
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. 2018]] — The plane-wave dual basis paper that first showed $\Theta(N^2)$ Hamiltonian terms and efficient qubitization.
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven 2019]] — First-quantized plane-wave approach with $\widetilde{O}(N^{1/3}\eta^{8/3})$ scaling. Discussed in Section 7 as a potentially competitive alternative for FeMoCo.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean et al. 2018]] — Givens rotation techniques for initial state preparation (cited for basis rotation of initial states under orbital optimization).
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes|Rubin, Babbush, McClean 2018]] — $n$-representability constraints for reducing $\lambda$ (cited as a technique to shrink the LCU 1-norm).
- Low & Chuang 2019 — [[Qubitization (Quantum Walk for Spectral Encoding)]] formalism.
- Low, Kliuchnikov, Schaeffer 2018 (arXiv:1812.00954) — The original QROAM (called "QRAM" there); this paper improves on their Toffoli counts.
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes|Reiher, Wiebe, Svore, Wecker, Troyer (2017)]] — The RWSWT FeMoCo benchmark ($\sim 10^{14}$ T gates for 108 spin-orbitals). This paper's primary comparison target.
- Li, Li, Dattani, Umrigar, Chan 2019 — The LLDUC active space (152 spin-orbitals) that this paper argues is more appropriate for FeMoCo.
- Gidney & Fowler 2019 — Surface code CCZ factory ($\sim 24$ qubitseconds per Toffoli at $d=31$).

---

## Cross-links

### Paper notes
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — foundational qubitization formalism (standard-form encoding + phased iterate) that this paper instantiates for arbitrary molecular orbital bases
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — the QSVT framework in which this algorithm fits: the block encoding here is an instance of the standard-form encoding abstraction
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Direct predecessor; all core techniques originate there
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — Source of the single/double factorization idea
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — Direct successor; THC replaces single factorization for further improvements
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — concurrent Babbush-Berry paper extending qubitization to random Gaussian-coupled Hamiltonians; uses a different PREPARE strategy (random circuits) targeting a physics rather than chemistry application
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — Plane-wave dual basis predecessor
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — Competing first-quantized approach discussed in Section 7
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — Applies Trotter to the same low-rank structure
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — Uses QROAM techniques from this paper
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — First-quantized qubitization also uses QROAM
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — $n$-representability technique for $\lambda$ reduction
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — Givens rotation technique cited for orbital optimization
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — Compression of factorized operators under unitary constraints
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — Questions whether FeMoCo-class problems require exponential quantum speedup; the $8.4 \times 10^{10}$ Toffoli estimate here is impressive but may target a problem solvable by classical heuristics at polynomial cost
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — Benchmarks single factorization qubitization against DF and THC on CYP P450; SF shows $O(N^{4.1})$ Toffoli scaling, worst of the three methods
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]] — Extends the sparse and low-rank block-encoding methods from this paper to force operators; shows $\lambda_F^{(\text{sparse})} \in O(N_H^{0.28})$ vs. $\lambda_H^{(\text{sparse})} \in O(N_H^{1.33})$ on H-chains
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — Extends the sparse and SF qubitization from this paper to periodic Bloch orbital systems; achieves $\sqrt{N_k}$ per-step speedup via [[Symmetry-Adapted Block Encoding via QROAM Data Reduction|symmetry-adapted QROAM]]
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar, Babbush, Rubin 2024]]

### Trick cards
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[QROM (Quantum Read-Only Memory)]]
- [[QROAM (Space-Time Tradeoff for QROM)]]
- [[Measurement-Based QROM Uncomputation]]
- [[Coherent Alias Sampling for PREPARE]]
- [[Unary Iteration]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Single Factorization for Qubitized Chemistry]]
- [[Sparse State Preparation for Qubitized Chemistry]]

See [[FeMoCo Resource Estimation Timeline]] for how this estimate fits in the broader progression.
