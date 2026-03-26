> **Source:** Nicholas C. Rubin, Dominic W. Berry, Fionn D. Malone, Alec F. White, Tanuj Khattar, A. Eugene DePrince III, Sabrina Sicolo, Michael Kühn, Michael Kaicher, Joonho Lee, Ryan Babbush, *Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals*, arXiv:2302.05531, PRX Quantum **4**, 040303 (2023)
> **Links:** [arXiv](https://arxiv.org/abs/2302.05531) · [Journal](https://doi.org/10.1103/PRXQuantum.4.040303)
> **Tags:** #quantum-simulation #materials #periodic-systems #qubitization #block-encoding #Bloch-orbitals #Gaussian-basis #tensor-hypercontraction #double-factorization #single-factorization #sparse #fault-tolerant #resource-estimation #LNO #surface-code #translational-symmetry

---

## The computational problem

Estimate ground-state energies of periodic crystalline materials described by the second-quantized electronic structure Hamiltonian in a local (Gaussian atom-centered) orbital basis, adapted to the translational symmetry of the lattice via Bloch orbitals. The Hamiltonian takes the form:

$$H = \sum_{\sigma,k} \sum_{pq} h_{pk,qk}\, a^\dagger_{pk\sigma} a_{qk\sigma} + \frac{1}{2} \sum_{\sigma,\tau} \sum_{Q,k,k'} \sum_{pqrs} V_{pk,q(k\ominus Q),r(k'\ominus Q),sk'}\, a^\dagger_{pk\sigma} a_{q(k\ominus Q)\sigma}\, a^\dagger_{r(k'\ominus Q)\tau} a_{sk'\tau}$$

where $p,q,r,s$ index bands (spatial orbitals) in the primitive cell, $k,k',Q$ label crystal momenta sampled on a Monkhorst-Pack grid of $N_k$ points in the Brillouin zone, and $\ominus$ denotes modular subtraction. The two-electron integrals have four-fold complex symmetry (not eight-fold as in the real molecular case), and translational symmetry enforces $k_p + k_r = k_q + k_s$ mod reciprocal lattice vector.

The goal: perform [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] phase estimation to extract eigenvalues to chemical precision ($\sim 1$ kcal/mol per unit cell), minimizing total Toffoli count and logical qubit count under fault-tolerant compilation.

---

## What the paper does

Extends four molecular [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] algorithms — sparse, [[Single Factorization for Qubitized Chemistry|single factorization]] (SF), [[Double Factorization of Two-Electron Integrals|double factorization]] (DF), and [[THC Non-Orthogonal Diagonal Coulomb Representation|tensor hypercontraction]] (THC) — from molecules to periodic systems using Bloch orbitals constructed from symmetry-adapted atom-centered Gaussians. The central question: can translational symmetry reduce the quantum resources compared to a brute-force supercell calculation at the $\Gamma$-point?

The answer is qualified. For sparse and SF, the symmetry-adapted block encodings achieve an $O(\sqrt{N_k})$ reduction in Toffoli cost per walk step, originating from the $N_k$-fold reduction in symmetry-unique data fed to [[QROAM (Space-Time Tradeoff for QROM)|QROAM]]. For DF, the walk-step cost also improves by $\sqrt{N_k}$, but $\lambda$ (the LCU 1-norm) worsens due to reduced variational freedom in the tensor compression — so the *total* Toffoli count shows no clear win. For THC, there is no asymptotic speedup at all: the cost of unary iteration over all $N_k N$ basis states is already $O(N_k N)$, which is a hard floor.

The paper also introduces a Bloch-orbital form of the THC factorization (k-THC), applies the algorithms to benchmark crystals (diamond, Si, BN, LiCl, AlN, Li, Al), and estimates resources for simulating the Lithium Nickel Oxide (LNO) cathode — a problem of genuine industrial interest for battery chemistry.

My assessment: this is a necessary and well-executed engineering paper. It does what needed doing — porting the Lee et al. 2021 toolkit to periodic systems — and is honest about the limitations. The $\sqrt{N_k}$ savings are real but modest; the larger story is that reaching the thermodynamic limit for condensed-phase systems remains wildly expensive even with the best algorithms. The LNO resource estimates are sobering: $O(10^{12})$ Toffolis for even small k-meshes with DF, and $O(10^{14}\text{–}10^{16})$ for anything approaching convergence. The gap between "interesting chemistry" and "feasible quantum computation" for materials is still enormous.

---

## The algorithm / construction

### Bloch orbitals from atom-centered Gaussians

Start with local Gaussian-type basis functions $\tilde{\chi}_p(\mathbf{r})$ centered on atoms in the unit cell. Periodize them:

$$\chi_{p,k}(\mathbf{r}) = \sum_{\mathbf{T}} e^{i\mathbf{k}\cdot\mathbf{T}}\, \tilde{\chi}_p(\mathbf{r} - \mathbf{T})$$

These are Bloch functions by construction. After a self-consistent field (Hartree-Fock or DFT) procedure, form molecular orbitals $\phi_{ik}(\mathbf{r}) = N_k^{-1/2} \sum_p c_{p,i}(k) \chi_{p,k}(\mathbf{r})$.

The one-electron integrals are nonzero only when $k_p = k_q$ (translational symmetry). The two-electron integrals $V_{pk_p, qk_q, rk_r, sk_s}$ are nonzero only when $k_p - k_q - (k_s - k_r) = \mathbf{G}$ (a reciprocal lattice vector). Introducing $Q = k_p \ominus k_q$, the Hamiltonian factors into momentum-conserving blocks.

The key structural difference from molecular systems: the integrals are complex-valued with only four-fold symmetry (not eight-fold), so $\lambda$ for the sparse LCU is a factor of 2 larger than in the molecular case.

### Qubitization framework (common to all four LCUs)

All four methods express $H = \sum_\ell \omega_\ell U_\ell$ as a linear combination of unitaries with non-negative weights $\omega_\ell$ and $\lambda = \sum_\ell \omega_\ell$. The phase estimation cost is:

$$\frac{\pi\lambda}{2\varepsilon_{\rm PEA}} \left(C_{\rm SELECT} + C_{\rm PREPARE} + C_{\rm PREPARE^\dagger} + \log L\right)$$

where the intensive quantity is $\lambda/N_k$ (energy per unit cell). The choice of LCU factorization affects all three quantities: $\lambda$, SELECT cost, and PREPARE cost.

### 1. Sparse representation

The Hamiltonian is directly mapped to Pauli operators via Jordan-Wigner. Under four-fold symmetry, $\lambda$ is:

$$\lambda = \lambda_{\tilde{H}_1} + \lambda_{H_2} = \sum_{k,p,q}\left(|{\rm Re}[h'_{pk,qk}]| + |{\rm Im}[h'_{pk,qk}]|\right) + \sum_{k,k',Q,p,q,r,s}\left(|{\rm Re}[V]| + |{\rm Im}[V]|\right)$$

PREPARE uses [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] on $O(N_k^3 N^4)$ symmetry-unique nonzero entries. With [[QROAM (Space-Time Tradeoff for QROM)|QROAM]], the cost is $\tilde{O}(N_k^{3/2} N^2)$ — a $\sqrt{N_k}$ improvement over the supercell's $\tilde{O}(N_k^2 N^2)$. SELECT applies Pauli strings at cost $O(N_k N)$, which is subdominant.

Practical issue: convergence of the nonzero integral count with $N_k$ is slow (the number of nonzero elements per block depends on $N_k$ at small grid sizes), making the $\sqrt{N_k}$ advantage hard to see for the system sizes studied.

### 2. Single factorization (SF)

Cholesky/density-fitting decomposition of the Coulomb tensor:

$$V_{pk_p,qk_q,rk_r,sk_s} = \sum_n L_{pk_p q k_q, n}\, L^*_{sk_s rk_r, n}$$

The two-body operator factors into squares of Hermitian one-body operators $\hat{A}_n(Q)$ and $\hat{B}_n(Q)$, implemented via oblivious amplitude amplification (which halves $\lambda$). The [[Coherent Alias Sampling for PREPARE|outer PREPARE]] over $Q,n$ has $M N_k + 1$ items; the inner PREPARE over $k,p,q$ has $O(N_k^2 N^3)$ total data. With QROAM: $\tilde{O}(N_k N^{3/2})$ Toffolis. The supercell needs $O(N_k^3 N^3)$ data, giving $\tilde{O}(N_k^{3/2} N^{3/2})$. The $\sqrt{N_k}$ advantage is clean and visible even at small $N_k$.

### 3. Double factorization (DF)

The $\hat{A}_n(Q)$ operators are further diagonalized per $k$ into rank-reduced rotated-basis number operators:

$$\hat{A}_n(Q) = \sum_k U_n^A(Q,k) \left(\sum_\sigma \sum_p f_p^A(Q,n,k)\, n_{pk\sigma}\right) U_n^A(Q,k)^\dagger$$

The basis rotations are implemented via [[Givens Rotation Slater Determinant Preparation|Givens rotations]] loaded from QROM. A new feature: controlled swaps between registers for $k$ and $k \ominus Q$ move the relevant part of the system register into $N$ working qubits. The total data to specify rotations is $\tilde{O}(N_k N^2 \Xi)$ where $\Xi$ is the average rank of the second factorization (expected $O(N_k N)$). Dominant cost: $\tilde{O}(N\sqrt{N_k \Xi})$ Toffolis for the QROM outputting rotation angles — a $\sqrt{N_k}$ improvement over the supercell.

However, $\lambda_{\rm DF}$ for the symmetry-adapted case is *larger* than for the supercell, because the supercell has more variational freedom in choosing non-orthogonal bases. This is a real and annoying effect: the per-step savings are eaten by more walk steps.

### 4. Tensor hypercontraction (THC / k-THC)

The [[THC Non-Orthogonal Diagonal Coulomb Representation|THC factorization]] is adapted to Bloch orbitals by decomposing the cell-periodic part of the density:

$$u^*_{pk_p}(\mathbf{r})\, u_{qk_q}(\mathbf{r}) \approx \sum_\mu \xi_\mu(\mathbf{r})\, u^*_{pk_p}(\mathbf{r}_\mu)\, u_{qk_q}(\mathbf{r}_\mu)$$

The central tensor becomes $\zeta_{\mu\nu}^{Q, G_{k,k-Q}, G_{k',k'-Q}}$, where $G$ are reciprocal lattice vectors (at most 8 distinct values). This gives $O(N_k M^2)$ central tensor elements instead of the $O(N_k^3 M^2)$ one might naively expect.

The block encoding uses [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] over $Q, G_1, G_2, \mu, \nu$ (outer PREPARE), then inner preparation of $k$ conditioned on $Q, G, \mu$. Basis rotations via [[QROM-Loaded Givens Rotation Networks|QROM-loaded Givens rotations]]. The cost bottleneck: unary iteration over the full $N_k N$ basis to apply $c^\dagger c$ operators, which is already $O(N_k N)$ and cannot be reduced by symmetry.

So THC shows no *asymptotic* speedup from symmetry adaptation. At finite sizes, there's an apparent $\sqrt{N_k}$ improvement in QROAM costs, but this is a finite-size artifact that vanishes as $N_k$ grows and unary iteration dominates.

The k-THC factors are obtained via interpolative separable density fitting (ISDF) followed by L1-regularized reoptimization (as in [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings et al. 2022]]). A THC rank of $c_{\rm THC} = 8$ (i.e., $M = 8 \times N/2$) gives MP2 errors below 0.1 mHa/cell.

---

## Key results

### Asymptotic scaling (Table II)

| Representation | Walk-step Toffolis (symmetry-adapted) | Walk-step Toffolis (supercell) | Speedup |
|---|---|---|---|
| Sparse | $\tilde{O}(N_k^{3/2} N^2 \lambda_{\rm sparse}/\varepsilon)$ | $\tilde{O}(N_k^2 N^2 \lambda_{\rm sparse,SC}/\varepsilon)$ | $\sqrt{N_k}$ |
| SF | $\tilde{O}(N_k N^{3/2} \lambda_{\rm SF}/\varepsilon)$ | $\tilde{O}(N_k^{3/2} N^{3/2} \lambda_{\rm SF,SC}/\varepsilon)$ | $\sqrt{N_k}$ |
| DF | $\tilde{O}(\sqrt{N_k} N \sqrt{\Xi}\, \lambda_{\rm DF}/\varepsilon)$ | $\tilde{O}(N_k N \sqrt{\tilde{\Xi}}\, \lambda_{\rm DF,SC}/\varepsilon)$ | $\sqrt{N_k}$ per step, but $\lambda_{\rm DF} > \lambda_{\rm DF,SC}$ |
| THC | $\tilde{O}(N_k N\, \lambda_{\rm THC}/\varepsilon)$ | $\tilde{O}(N_k N\, \lambda_{\rm THC,SC}/\varepsilon)$ | None asymptotically |

### Diamond resource estimates (Table III, cc-pVDZ, 52 spin-orbitals per primitive cell)

| LCU | k-mesh | Toffolis | Logical qubits | Physical qubits (M) | Runtime (days) |
|---|---|---|---|---|---|
| DF | [1,1,1] | $9.6 \times 10^8$ | 2,396 | 1.55 | 0.18 |
| DF | [2,2,2] | $6.7 \times 10^{10}$ | 18,693 | 18.5 | 12.7 |
| DF | [3,3,3] | $1.1 \times 10^{12}$ | 68,470 | 82.4 | 237 |
| THC | [1,1,1] | $1.7 \times 10^{10}$ | 18,095 | 14.2 | 3.1 |
| THC | [2,2,2] | $4.9 \times 10^{11}$ | 36,393 | 35.6 | 105 |

DF dominates at all tested system sizes. THC looks competitive at small $N_k$ but its classical preprocessing (reoptimizing THC factors) is prohibitively expensive for larger systems.

### LNO resource estimates (Table VI, GTH-DZVP, 116 spin-orbitals per primitive cell for R$\bar{3}$m)

| LCU | Structure | k-mesh | Toffolis | Logical qubits | Runtime (days) |
|---|---|---|---|---|---|
| DF | R$\bar{3}$m | [2,2,2] | $5.0 \times 10^{12}$ | 149,939 | 1,080 |
| DF | R$\bar{3}$m | [3,3,3] | $7.3 \times 10^{13}$ | 598,286 | 17,900 |
| DF | C2/m | [2,2,1] | $1.2 \times 10^{12}$ | 75,178 | 256 |
| DF | P21/c | [1,2,1] | $1.3 \times 10^{12}$ | 75,383 | 276 |

Even at the smallest useful k-meshes, these are year-scale runtimes. Convergence to the thermodynamic limit requires at least [3,3,3] or [4,4,4] grids, pushing into $10^{14}\text{–}10^{16}$ Toffoli territory.

---

## Comparison with prior work

| Method | Basis | Representation | Best scaling | Periodic? | Exploits $k$-symmetry? |
|---|---|---|---|---|---|
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. 2018]] | Plane-wave dual | Trotter / LCU | $\tilde{O}(N^{8/3})$ depth | Yes | Implicitly (diagonal) |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al. 2018]] | Plane-wave dual | Qubitization | $O(N^3/\varepsilon)$ T | Yes | Implicitly (diagonal) |
| [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes\|Su, Berry et al. 2021]] | First-quant. plane-wave | Qubitization | $\tilde{O}(\eta^{4/3} N^{2/3}/\varepsilon)$ | Yes | Yes |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes\|Lee, Berry et al. 2021]] | Gaussian MO | THC qubitization | $\tilde{O}(N\lambda_\zeta/\varepsilon)$ | No (molecules) | N/A |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry, Gidney et al. 2019]] | Gaussian MO | Sparse / SF qubitization | $\tilde{O}(N^{3/2}\lambda/\varepsilon)$ | No (molecules) | N/A |
| Ivanov et al. 2022 (arXiv:2210.02403) | Local orbitals | Sparse qubitization | — | Yes | Partial |
| **This paper** | Bloch orbitals (Gaussian) | Sparse/SF/DF/THC qubitization | DF: $\tilde{O}(\sqrt{N_k} N\sqrt{\Xi}\lambda/\varepsilon)$ | **Yes** | **Yes** (all four LCUs) |

The paper fills a genuine gap: prior plane-wave methods can't describe localized phenomena (catalysis, cusps) efficiently, while prior Gaussian-orbital methods didn't handle periodicity. The Bloch orbital approach inherits the compact basis of Gaussian methods while recovering $O(\sqrt{N_k})$ from translational symmetry — at least in the per-step cost.

But the first-quantized plane-wave approach of [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. 2021]] has fundamentally better scaling in basis size ($\eta^{4/3} N^{2/3}$ vs. $N_k N$ for THC or $\sqrt{N_k} N \sqrt{\Xi}$ for DF). For systems where plane waves converge (extended solids without sharp cusps), first quantization is still the better asymptotic bet. The Bloch orbital approach wins when you need a small, chemically meaningful basis — e.g., describing $d$-electron correlation in transition metal oxides where plane waves would need $N = O(10^6)$ to converge.

---

## Limits / caveats

1. **$\lambda$ worsens with symmetry for DF and THC.** The reduced variational freedom in the symmetry-adapted tensor compression increases $\lambda$, eating into the per-step savings. This is a structural limitation, not an error.

2. **THC classical preprocessing is prohibitive.** Reoptimizing THC factors to minimize $\lambda$ (as done in [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee et al. 2021]] and [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings et al. 2022]]) doesn't scale to large $N_k$. So while THC gives competitive Toffoli counts at small sizes, it can't be deployed for the large-$N_k$ calculations needed for thermodynamic limit convergence.

3. **Thermodynamic limit convergence is the bottleneck.** Correlated calculations need at least [3,3,3] and ideally [4,4,4] k-meshes with triple-zeta basis sets to converge materials properties. The resource estimates at these sizes are $10^{14}\text{–}10^{16}$ Toffolis — many orders of magnitude beyond near-term feasibility.

4. **Classical methods also struggle.** The paper's own CCSD and ph-AFQMC calculations on LNO couldn't converge to the thermodynamic limit either. This isn't a quantum-computer-specific failure — it's a fundamental difficulty of correlated condensed-phase simulation. But it does mean the quantum advantage case rests on *eventual* hardware scaling, not current capabilities.

5. **Four-fold vs. eight-fold symmetry.** Complex-valued integrals in the periodic setting give only four-fold symmetry, doubling $\lambda$ for the sparse LCU relative to the molecular case. This structural penalty is unavoidable for periodic systems with time-reversal-broken orbitals.

6. **Space group symmetry unexploited.** The paper only uses translational (Abelian) symmetry. Point group symmetries could provide additional savings but are left to future work.

---

## Reusable ideas

1. [[Symmetry-Adapted Block Encoding via QROAM Data Reduction]] — The core technique: reduce the data loaded by QROAM in PREPARE by a factor of $N_k$ (exploiting translational symmetry of the integrals), giving $\sqrt{N_k}$ savings in Toffoli cost. Applicable to any Abelian symmetry group.

2. [[k-Point THC Factorization (Bloch Orbital THC)]] — Tensor hypercontraction adapted to periodic systems by factorizing the cell-periodic part of Bloch orbital densities; central tensor indexed by momentum transfer $Q$ and reciprocal lattice vectors $G$ instead of four independent momenta.

3. [[Controlled Momentum-Register Swaps for Periodic Block Encodings]] — Using $k$ and $k \ominus Q$ registers to control swaps of system qubits into working registers for basis rotations. This coherent data movement is the dominant cost floor ($O(N_k N)$) that limits the achievable speedup.

---

## References within this paper

Key cited works with vault notes:
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. 2021]] — the molecular THC qubitization this paper extends (Ref. [32])
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney et al. 2019]] — molecular sparse/SF qubitization (Ref. [71])
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]] — qubitization framework, unary iteration, QROM (Ref. [70])
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush et al. 2018]] — plane-wave dual basis for materials (Ref. [4])
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry et al. 2021]] — first-quantized plane-wave compilation for materials (Ref. [3])
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings, Babbush, Rubin et al. 2022]] — L1-regularized THC optimization (Ref. [37])
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Rubin, Lee, Babbush 2021]] — variational tensor compression (Ref. [107])
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta et al. 2018]] — double factorization Trotter (not directly cited but the DF approach originates here)

Key cited works without vault notes:
- Ivanov et al. 2022 (arXiv:2210.02403) — first steps toward symmetry-adapted sparse block encoding for periodic systems; this paper provides an alternative derivation and extends to SF, DF, THC
- von Burg et al. 2021 (PRR **3**, 033055) — double factorization qubitization for molecules (Ref. [36])
- Low, Kliuchnikov, Schaeffer 2018 (arXiv:1812.00954) — QROAM construction (Ref. [76])
- Oumarou et al. 2022 (arXiv:2212.07957) — variational $\lambda$ minimization for THC (Ref. [79])
- Ye and Berkelbach 2021 — range-separated density fitting for periodic integrals (Ref. [60])

---

## Cross-links

### Paper notes
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — direct predecessor for molecular THC qubitization
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — molecular sparse/SF qubitization
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — qubitization framework
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — plane-wave dual basis for materials (alternative basis choice)
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — first-quantized competitor for periodic systems
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — Trotter-based alternative for periodic systems
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — double factorization origin
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — L1-regularized THC optimization technique
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — variational tensor compression
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — context on quantum advantage feasibility for chemistry
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — First-quantized competitor with GTH pseudopotentials; direct comparison on LNO: ~12× more Toffolis but ~50× fewer qubits; spacetime volume favours first quantisation
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar, Babbush, Rubin 2024]]
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al 2023]]
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — the real-space first-quantized precursor; Kivlichan et al. (2017) established the controlled swap network for SELECT and the $\tilde{O}(\eta^2)$ oracle model that motivates first-quantized approaches to materials simulation
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]] — the DG basis compresses the plane-wave dual (used in second-quantized materials simulation) toward a block-local form; complementary to this paper's Bloch orbital approach: DG reduces integral count from $O(N^4)$ to $O(N^2)$, while Bloch orbitals exploit translational symmetry to cut QROAM cost by $\sqrt{N_k}$

### Trick cards
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the simulation framework
- [[Coherent Alias Sampling for PREPARE]] — state preparation primitive used in all four LCUs
- [[QROAM (Space-Time Tradeoff for QROM)]] — the dominant cost primitive; symmetry savings originate here
- [[QROM (Quantum Read-Only Memory)]] — underlying data loading
- [[Single Factorization for Qubitized Chemistry]] — one of the four LCU representations
- [[Double Factorization of Two-Electron Integrals]] — another LCU representation
- [[THC Non-Orthogonal Diagonal Coulomb Representation]] — molecular THC trick, extended here to periodic systems
- [[Givens Rotation Slater Determinant Preparation]] — basis rotation circuit used in DF and THC
- [[QROM-Loaded Givens Rotation Networks]] — rotation angle loading
- [[Unary Iteration]] — fundamental primitive for SELECT
- [[Symmetry-Adapted Block Encoding via QROAM Data Reduction]] — new trick from this paper
- [[k-Point THC Factorization (Bloch Orbital THC)]] — new trick from this paper
- [[Controlled Momentum-Register Swaps for Periodic Block Encodings]] — new trick from this paper
- [[Fermionic Fast Fourier Transform (FFFT)]] — related technique for plane-wave basis changes
- [[Plane-Wave Dual Basis]] — alternative basis for periodic systems
