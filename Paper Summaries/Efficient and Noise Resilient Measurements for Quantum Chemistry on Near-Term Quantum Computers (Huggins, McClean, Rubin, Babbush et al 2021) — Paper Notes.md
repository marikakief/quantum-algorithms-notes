> **Source:** William J. Huggins, Jarrod R. McClean, Nicholas C. Rubin, Zhang Jiang, Nathan Wiebe, K. Birgitta Whaley, Ryan Babbush, *Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers*, arXiv:1907.13117, npj Quantum Information **7**, 23 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/1907.13117) · [Journal](https://doi.org/10.1038/s41534-020-00341-7)
> **Tags:** #VQE #measurement #NISQ #quantum-chemistry #low-rank #double-factorization #error-mitigation #postselection #Givens-rotation

---

## The computational problem

In [[Variational Quantum Eigensolver (VQE)|VQE]], estimating the ground-state energy of a molecular Hamiltonian requires measuring many terms. The standard approach decomposes $H$ into $O(N^4)$ Pauli strings, with total shot count bounded by

$$M \leq \left(\frac{\sum_\ell |\omega_\ell|}{\varepsilon}\right)^2$$

where $\omega_\ell$ are the Pauli coefficients and $\varepsilon$ is the target precision. Prior assessments using this bound concluded that VQE measurement times for non-trivial molecules are "astronomically large." The question is whether a smarter decomposition of the Hamiltonian — one that exploits the structure of the two-electron integrals rather than treating them as generic Pauli sums — can dramatically reduce the actual number of circuit repetitions.

## What the paper does

The paper introduces **Basis Rotation Grouping**: use the [[Double Factorization of Two-Electron Integrals|eigendecomposition of the two-electron integral tensor]] to rewrite the Hamiltonian in a form where all terms within each factor are simultaneously diagonal (number operators $n_p$ and products $n_p n_q$), then measure each factor by applying a single-particle basis rotation before computational-basis measurement. This yields only $L + 1 = O(N)$ distinct measurement circuits instead of $O(N^4)$ Pauli-grouping circuits — a **quartic reduction** in term groupings over naive approaches and a **cubic reduction** over the best prior Pauli-grouping methods. Numerically, the actual variance (not just group count) shows up to **three orders of magnitude** fewer circuit repetitions for modest systems (24 qubits), with evidence of asymptotic improvement: total measurements scale as $\sim N^{2.75}$ for H-chains, compared to $\sim N^{4.9}$ for Pauli-grouping methods.

As a bonus, measuring only in diagonal (number-operator) form makes the scheme naturally resilient to readout errors and enables **postselection on particle number $\eta$ and spin $S_z$** — not just their parities — at zero additional circuit cost.

## The algorithm / construction

### Hamiltonian factorization

Start from the standard second-quantized electronic structure Hamiltonian and write it in a factored form:

$$H = U_0 \left(\sum_p g_p\, n_p\right) U_0^\dagger + \sum_{\ell=1}^{L} U_\ell \left(\sum_{pq} g^{(\ell)}_{pq}\, n_p n_q\right) U_\ell^\dagger$$

where $n_p = a_p^\dagger a_p$ and each $U_\ell$ implements a single-particle basis change:

$$U = \exp\!\left(\sum_{pq} \kappa_{pq}\, a_p^\dagger a_q\right)$$

This decomposition follows from eigendecomposing the two-electron integral tensor $V_{pqrs}$ as a matrix indexed by $(pq, rs)$:

$$V_{pqrs} = \sum_\ell w_\ell\, v^{(\ell)}_{pq}\, v^{(\ell)}_{rs}$$

then diagonalizing each one-body operator $\sum_{pq} v^{(\ell)}_{pq} a_p^\dagger a_q$ to obtain rotated number-number interactions. This is the same [[Double Factorization of Two-Electron Integrals|double factorization]] used for Trotter compilation in [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta et al. (2018)]], but repurposed here for measurement rather than time evolution.

The rank $L$ is $O(N)$ for molecular Hamiltonians in arbitrary basis sets — this is empirically well-established from the classical quantum chemistry literature on Cholesky decomposition and density fitting. For special bases like the [[Plane-Wave Dual Basis]], $L = 1$.

### Measurement protocol

For each factor $\ell \in \{0, 1, \ldots, L\}$:

1. Prepare the variational state $|\psi(\theta)\rangle$.
2. Apply the basis rotation circuit $U_\ell^\dagger$ (implemented via [[Givens Rotation Slater Determinant Preparation|Givens rotations]] on a linear chain: $N^2/4 - N/2$ two-qubit gates, depth $N/2$).
3. Measure all qubits in the computational basis.
4. From the bitstring outcomes, extract all $\langle n_p \rangle_\ell$ and $\langle n_p n_q \rangle_\ell$ simultaneously.

The energy estimator is:

$$\langle H \rangle = \sum_p g_p \langle n_p \rangle_0 + \sum_{\ell=1}^{L} \sum_{pq} g^{(\ell)}_{pq}\, \langle n_p n_q \rangle_\ell$$

### Shot allocation

Measurements are distributed across the $L+1$ groups according to optimal allocation (Eq. 5 from [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes|Rubin, Babbush, McClean (2018)]]). In practice, the optimal fractions are approximated using a classically tractable CISD calculation. The paper shows this approximation introduces $< 3\%$ overhead across all systems studied.

### Error mitigation by postselection

Since every measurement is in the number basis (after the rotation), each shot simultaneously measures the total particle number $\eta = \sum_p n_p$ and spin $S_z$. Shots where the observed $\eta$ or $S_z$ deviates from the target are discarded. This is strictly stronger than parity-based postselection (which only checks $\eta \bmod 2$): it catches single-particle errors, not just pairs.

The postselection cost is $\sim 1/\mathrm{Tr}(P\rho)$ additional measurements, where $P$ projects onto the correct symmetry sector. In practice this factor is partially offset because postselection removes high-variance outlier measurements from wrong particle-number sectors.

### Readout error resilience

Under Jordan-Wigner, standard Pauli-term measurement requires reading out operators with support on up to $N$ qubits. A symmetric bitflip channel with rate $p$ suppresses the expectation value by $(1-2p)^K$ for a $K$-qubit operator. In the Basis Rotation Grouping scheme, all measured operators have support on at most 2 qubits ($n_p n_q$), so the suppression is at most $(1-2p)^2$ — independent of system size. This is an exponential improvement in noise resilience.

## Key results

**Measurement group count:**

$$\text{Number of distinct circuits} = L + 1 = O(N)$$

vs $O(N^4)$ for naive Pauli decomposition and $O(N^3)$ for the best prior grouping methods.

**Empirical variance scaling (H-chain data):**

| Measurement strategy | Fitted exponent $b$ in $M \sim N^b$ |
|---|---|
| Bound from qubit Hamiltonian | $4.90 \pm 0.02$ |
| Separate measurements | $5.70 \pm 0.06$ |
| Pauli word grouping | $4.88 \pm 0.06$ |
| RDM constraints | $5.63 \pm 0.06$ |
| **Basis Rotation Grouping** | $\mathbf{2.75 \pm 0.01}$ |

**Concrete example:** For H₆ in 6-31G (24 qubits), the commonly cited bound suggests ~55 days of measurement at 10 kHz repetition rate for chemical accuracy. Basis Rotation Grouping requires **44 minutes**.

**Gate overhead per measurement circuit:** $N^2/4 - N/2$ two-qubit gates (Givens rotations), depth $N/2$, linear connectivity only.

## Comparison with prior work

| Method | # Partitions | Gate count | Depth | Connectivity | Diagonal? |
|---|---|---|---|---|---|
| Separate Pauli terms | $O(N^4)$ | $N$ | 1 | any | no |
| Compatible Pauli heuristic | $O(N^4)$ | $N$ | 1 | any | no |
| [[Variance Reduction via N-Representability Constraints (LP Method)|RDM constraints]] | $O(N^4)$ | $N$ | 1 | any | no |
| Commuting Pauli clique cover | $O(N^3)$ | $O(N^2)$ | — | full | no |
| Anticommuting Pauli clique cover | $O(N^3)$ | $O(N^2 \log N)$ | — | full | no |
| [[Fermionic Swap Network]] generalization | $O(N^3)$ | $O(N^2)$ | $O(N)$ | linear | no |
| **Basis Rotation Grouping (this paper)** | $O(N)$ | $N^2/4$ | $N/2$ | **linear** | **yes** |

The "diagonal" column is significant: only diagonal (number-operator) measurements enable direct postselection on $\eta$ and $S_z$ eigenvalues. All other methods measure non-diagonal Pauli operators, limiting them to parity-based error mitigation at best.

## Limits / caveats

1. **Gate overhead:** Each measurement circuit adds $O(N^2)$ gates in a depth-$N/2$ Givens rotation network. For very noisy hardware where every gate matters, this additional depth may introduce more error than it mitigates. The paper's noise simulations (Figs 3–4) show this tradeoff is favorable for gate error rates $\lesssim 10^{-2}$, but it depends on the specific hardware.

2. **Not obviously helpful for small systems:** For H₂O in the minimal STO-3G basis, Basis Rotation Grouping shows no advantage over Pauli word grouping. The gains emerge with larger basis sets and larger systems, particularly as one approaches the thermodynamic limit.

3. **Variance analysis, not worst-case bounds:** The paper's main results are computed using exact FCI ground-state variances. These are numerical demonstrations, not proven asymptotic bounds. The $N^{2.75}$ scaling is empirically fitted on H-chains — there is no theorem guaranteeing this exponent for general molecules.

4. **Classical preprocessing:** The optimal shot allocation requires computing variances from a CISD calculation. This is classically polynomial but adds to the overall workflow. The paper shows the approximation error is negligible ($< 3\%$).

5. **Rank $L$:** For general basis chemistry, $L = O(N)$ is empirically observed but basis-dependent. The favorable scaling is well-established in the classical Cholesky decomposition literature, but pathological cases could in principle have higher rank.

6. **Covariance structure:** The paper argues that the favorable variance scaling is partly due to beneficial covariances between terms within each diagonal group. This is empirically demonstrated, but there's no analytical proof that the covariance structure remains favorable for arbitrary molecules or excited states.

## Reusable ideas

1. **[[Basis Rotation Grouping for VQE Measurement]]** — Factorize the two-electron integral tensor, apply single-particle basis rotations before measurement, measure all terms in a given factor simultaneously as diagonal number operators. Reduces measurement circuits from $O(N^4)$ to $O(N)$ for molecular Hamiltonians.

2. **[[Number-Basis Postselection for Error Mitigation]]** — When all measurements are diagonal in the number basis (after appropriate rotations), postselect on the correct eigenvalues of $\eta$ and $S_z$ at zero additional circuit cost. Strictly stronger than parity-based postselection.

3. **[[Qubit vs Fermionic Variance Bounds]]** — Computing measurement cost bounds from the qubit (Jordan-Wigner) Hamiltonian gives substantially tighter estimates than computing them from the fermionic representation. The gap arises from sign cancellations when Jordan-Wigner maps symmetric integral tensor elements into Pauli operators. The bound from qubit coefficients is ~$3\times$ tighter than the fermionic bound for the systems studied. Implication: fault-tolerant cost estimates should be (re-)computed using qubit-representation coefficients.

## References within this paper

- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes|Rubin, Babbush, McClean (2018)]] — The RDM constraint / $n$-representability approach to variance reduction that this paper benchmarks against. The optimal shot allocation formula (Eq. 5) also comes from here.
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al. (2018)]] — The [[Double Factorization of Two-Electron Integrals|double factorization]] that provides the Hamiltonian decomposition used here, originally developed for Trotter compilation.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean et al. (2018)]] — The [[Givens Rotation Slater Determinant Preparation|Givens rotation network]] for implementing single-particle basis changes on a linear chain, which provides the measurement circuits.
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]] — The [[Plane-Wave Dual Basis]] where $L=1$, and the [[Variational Measurement Reduction via Dual Basis]] idea that this paper generalizes to arbitrary molecular orbital bases.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — Early VQE experiment referenced for context; the [[Pauli Expectation Value Estimation]] baseline this paper improves upon.
- Wecker, Hastings, Troyer (2015) — The source of the $(\sum_\ell |\omega_\ell|/\varepsilon)^2$ measurement bound that this paper demonstrates is overly pessimistic.
- Verteletskyi, Yen, Izmaylov (2020) — Minimum clique cover for Pauli grouping; achieves $O(N^3)$ groups with single-qubit rotations.
- O'Gorman, Huggins, Rieffel, Whaley (2019) — Generalized swap networks that achieve $O(N^3)$ groups using $O(N^2)$ gates on linear connectivity.
- Clements, Humphreys, Metcalf, Kolthammer, Walmsley (2016) — Multiport interferometry technique that enables halving the Givens rotation depth from $N$ to $N/2$.
- Bonet-Monroig et al. (2018), McArdle, Yuan, Benjamin (2019) — Symmetry verification and zero-noise extrapolation error mitigation strategies that complement the postselection approach proposed here.

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — Original VQE proposal whose $O(N^4)$ Pauli measurement baseline this paper reduces to $O(N)$ measurement circuits
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]] — Introduces commuting-group measurement and correlated sampling strategies; this paper supersedes those with a factorization-based approach giving a far larger reduction
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — direct predecessor on measurement reduction; benchmarked against in this paper
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — source of the double factorization used here
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — source of the Givens rotation circuits
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — plane-wave dual basis measurement as a special case ($L=1$)
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — VQE baseline
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]] — alternative (greedy sum-of-squares) factorization that could also be used for measurement grouping
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC gives another factorization; its measurement implications extend this work
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — same lead author (Huggins); builds on the measurement insights here for shadow tomography in QC-QMC
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] — same lead author; addresses the same multi-observable estimation problem in the fault-tolerant/high-precision regime using [[Gradient Encoding for Multi-Observable Estimation|quantum gradient estimation]] rather than sampling
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]] — applies BRG and fermionic shadow tomography to force vector estimation; shows force estimation costs roughly the same as energy estimation in NISQ when shots are optimised via [[Parallelised Importance Sampling for Vector Estimation|parallelised importance sampling]]
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — another VQE measurement cost analysis
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] — VQSE requires RDM measurements; basis rotation grouping could reduce their cost
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes|McClean, Jiang, Rubin, Babbush, Neven 2019]]
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes|Huang, Babbush, McClean et al 2021]]
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|Wan, Huggins, Lee, Babbush 2022]]
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes|Tazhigulov, Sun, Babbush, Chan et al 2022]]
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes|King, Gosset, Kothari, Babbush 2024]]

### Trick cards
- [[Basis Rotation Grouping for VQE Measurement]] — the central trick of this paper
- [[Number-Basis Postselection for Error Mitigation]] — the error mitigation strategy
- [[Qubit vs Fermionic Variance Bounds]] — the representation-dependent bound gap
- [[Double Factorization of Two-Electron Integrals]] — the tensor decomposition underlying the grouping
- [[Givens Rotation Slater Determinant Preparation]] — the circuit primitive for basis rotations
- [[Fermionic Swap Network]] — related measurement approach from O'Gorman et al.
- [[Pauli Expectation Value Estimation]] — the naive baseline this improves upon
- [[Variational Quantum Eigensolver (VQE)]] — the algorithmic framework
- [[Variance Reduction via N-Representability Constraints (LP Method)]] — the RDM constraint method benchmarked against
- [[Variational Measurement Reduction via Dual Basis]] — the plane-wave special case ($L=1$)
- [[Partial Basis Rotation (Reduced Givens Count)]] — optimization when the basis rotation has low rank
