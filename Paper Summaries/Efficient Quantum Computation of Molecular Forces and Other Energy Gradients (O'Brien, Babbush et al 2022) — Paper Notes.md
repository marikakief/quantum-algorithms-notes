> **Source:** Thomas E. O'Brien, Michael Streif, Nicholas C. Rubin, Raffaele Santagati, Yuan Su, William J. Huggins, Joshua J. Goings, Nikolaj Moll, Elica Kyoseva, Matthias Degroote, Christofer S. Tautermann, Joonho Lee, Dominic W. Berry, Nathan Wiebe, Ryan Babbush, *Efficient Quantum Computation of Molecular Forces and Other Energy Gradients*, arXiv:2111.12437, Physical Review Research **4**, 043210 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2111.12437) · [Journal](https://doi.org/10.1103/PhysRevResearch.4.043210)
> **Tags:** #quantum-chemistry #force-estimation #energy-gradients #molecular-dynamics #block-encoding #fault-tolerant #NISQ #measurement #Hellmann-Feynman #finite-difference #overlap-estimation #gradient-estimation

---

## The computational problem

Given the Born-Oppenheimer electronic Hamiltonian $H(R)$ parametrised by nuclear positions $R$, compute the energy gradient $dE/dR$ — the $3N_a$-dimensional vector of forces on the $N_a$ nuclei. Force estimation is the bottleneck for molecular dynamics (MD), geometry optimisation, and computing molecular properties (dipole moments, NMR coupling constants, etc.) via derivatives of the energy with respect to external perturbations.

The formal task: estimate the gradient vector $dE/dR$ to within $\varepsilon$ in 2-norm. The target precision for MD is about 6.4 mHa/Å (RMS per force component) based on reproducing the radial distribution function of water; for geometry optimisation, about 0.6 mHa/Å.

Two estimation strategies:
1. **Hellmann-Feynman (HF) approach:** Compute $\langle \psi | dH/dR_i | \psi \rangle$ directly. The force operator $dH/dR_i$ is a Hermitian observable that can be block-encoded and measured.
2. **Finite difference (FD) approach:** Estimate energies $E(R + \delta)$ at nearby configurations and use higher-order central differences.

## What the paper does

This is the first systematic costing of force estimation on quantum computers, covering both NISQ and fault-tolerant settings. The headline results:

- **NISQ:** Force vector estimation (to fixed 2-norm) costs roughly the same as energy estimation — possibly cheaper by a constant factor. Optimised tomography schemes (parallelised importance sampling, [[Basis Rotation Grouping for VQE Measurement|basis rotation grouping]], fermionic shadow tomography) reduce circuit repetitions by orders of magnitude over naive approaches.
- **Fault-tolerant:** Three Heisenberg-limited algorithms are developed and compared: (1) higher-order finite differences using phase estimation as a subroutine, (2) the overlap estimation algorithm (OEA) for direct expectation value estimation, and (3) the [[Gradient Encoding for Multi-Observable Estimation|gradient-based expectation value estimation]] algorithm from [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins et al. (2022)]].
- **Key finding:** The cost of block-encoding a force operator is at most a constant or polylog factor above the cost of block-encoding the Hamiltonian. For [[THC Non-Orthogonal Diagonal Coulomb Representation|THC]] encodings, the circuit cost grows by $\sim\sqrt{2}\times$ and the rescaling factor $\lambda_F$ by $\leq 2\times$. For plane-wave first-quantised systems, $\lambda_F \in O(\eta N^{2/3}/\Omega^{2/3})$ — the same asymptotic scaling as $\lambda_H$.
- **Surprising conclusion:** In fault tolerance, force estimation is *not* cheaper than energy estimation (unlike NISQ). All three FT methods are dominated by [[Hamiltonian simulation]] costs — either directly (finite differences require repeated phase estimation) or via the ground-state reflection operator (OEA and gradient-based methods). The gradient-based algorithm strictly dominates OEA by $N_a^{1/2}$, and in some regimes finite differences are competitive or better (when state preparation is cheap).
- **Practical outlook:** Single-point force estimation is feasible at roughly the same cost as energy estimation. But MD requires $10^6$–$10^9$ force evaluations, each taking hours on a fault-tolerant device. MD on a quantum computer is not tractable with current approaches. Geometry optimisation and property estimation (dipoles, coupling constants) are more promising near-term targets.

My assessment: This paper is a careful, honest accounting of what quantum computers can and cannot do for molecular dynamics. The "forces are basically free in NISQ" result is encouraging but largely academic given VQE's other problems. The FT analysis is where the real value lies: it identifies the ground-state reflection as the fundamental bottleneck and shows that the Huggins et al. gradient method is optimal for this task. The pessimistic conclusion about MD is important — it steers the field away from an overhyped application and toward geometry optimisation, where the quantum advantage story is more plausible.

## The algorithm / construction

### NISQ: Optimised tomography for force vectors

The key insight for NISQ is that all $3N_a$ force components share the same state $|\psi\rangle$, so measurements can be **parallelised** across force components. The paper optimises shot allocation by targeting the 2-norm of the error vector $\|\varepsilon\|_2$ rather than individual component errors:

$$M \leq \varepsilon^{-2} \left(\sum_j \sqrt{\sum_i \bar{\sigma}_{i,j}^2}\right)^2$$

where $\bar{\sigma}_{i,j}$ bounds the variance of estimating the $i$-th force component from the $j$-th basis rotation. Parallelisation saves up to a factor of $3N_a$ over separate estimation of each component.

Three measurement strategies are compared:
1. **Pauli measurements** with importance sampling (parallelised vs. separate)
2. **Fermionic shadow tomography** — random [[Basis Rotation Grouping for VQE Measurement|fermionic Gaussian Clifford]] unitaries, exact variance bounds from shadow norms
3. **[[Basis Rotation Grouping for VQE Measurement|Basis rotation grouping]]** via [[Double Factorization of Two-Electron Integrals|double factorization]] of the force operator

Numerically on H-chains (STO-6G, localised orbitals), fermionic shadows give the best scaling: $\Gamma_2^{(\text{FS})} \in O(N_a^{2.84})$ for force estimation vs. comparable scaling for the Hamiltonian. Parallelisation improves naive Pauli measurement cost from $O(N_a^{3.89})$ to $O(N_a^{3.69})$.

### Block encoding force operators

For second-quantised local basis sets, the force operator (Eq. 22 in the paper) is:

$$\frac{dH}{dR_A} = \sum_{pq} a_p^\dagger a_q \left[\frac{dh_{pq}}{dR_A} - \sum_m h_{mq} \frac{dS_{mp}}{dR_A}\right] + \sum_{pqrs} a_p^\dagger a_r^\dagger a_q a_s \left[\frac{dg_{pqrs}}{dR_A} - 2\sum_t g_{tqrs} \frac{dS_{tp}}{dR_A}\right]$$

where the terms involving $dS/dR_A$ are the **Pulay forces** from the nuclear-position-dependent basis.

**THC block encoding:** Differentiate the THC factorisation $G_{pqrs} = \sum_{\mu\nu} \chi_p^{(\mu)} \chi_q^{(\mu)} \zeta_{\mu\nu} \chi_r^{(\nu)} \chi_s^{(\nu)}$. The derivative introduces five terms instead of one, each with the same diagonal structure. Circuit cost: $T_F \sim \sqrt{2}\, T_H$. Rescaling factor: $\lambda_{F_i} = \sum_{\mu\nu} (|\zeta_{\mu\nu}| + \frac{1}{2}|d\zeta_{\mu\nu}/dR_i|)$.

**[[Double Factorization of Two-Electron Integrals|Low-rank factorisation]]:** Differentiate $V = \sum_l W^{(l)} W^{(l)\dagger}$ via the identity $dV/dR_i = \frac{1}{2}\sum_l [(W^{(l)} + dW^{(l)}/dR_i)^2 - (W^{(l)} - dW^{(l)}/dR_i)^2]$. Circuit cost: $T_F \sim O(\sqrt{NL})$ — same as the Hamiltonian. Rescaling factor: $\lambda_{F_i} = \frac{1}{2}\sum_l [(\sum_p |f_p^{(l+)}|)^2 + (\sum_p |f_p^{(l-)}|)^2]$, with $\lambda_F \sim 4\lambda_H$ in the worst case.

**First-quantised plane waves:** The force operator is a **one-body operator** (no Pulay force, since plane-wave overlap is $\delta_{pq}$), diagonal under the FFFT/QFT:

$$\frac{dH}{dR_{l,\alpha}} = \text{QFT}\left(-\frac{4\pi i Z_l}{\Omega} \sum_{i=1}^{\eta} \sum_{p,s \in G_0} \frac{k_s\, e^{ik_s \cdot (R_l - r_p)}}{|k_s|^2} |p\rangle\langle p|_i\right) \text{QFT}^\dagger$$

This gives $\lambda_{F_{l,\alpha}} \in O(\eta N^{2/3}/\Omega^{2/3})$ — same asymptotic scaling as $\lambda_H$. Block-encoding cost: $T_F = O(\eta \log N + \log N \log(N/\varepsilon))$.

**Numerical results:** On H-chains, $\lambda_F^{(\text{sparse})} \in O(N_H^{0.28})$ and $\lambda_F^{(\text{DF})} \in O(N_H^{0.06})$, compared to $\lambda_H^{(\text{sparse})} \in O(N_H^{1.33})$ and $\lambda_H^{(\text{DF})} \in O(N_H^{1.94})$. Forces are *much* cheaper to block-encode than the Hamiltonian. On water clusters, a similar trend: $\lambda_F^{(\text{sparse})} \in O(N_{\text{H}_2\text{O}}^{0.43})$ vs. $\lambda_H^{(\text{sparse})} \in O(N_{\text{H}_2\text{O}}^{1.37})$.

### FT Algorithm 1: Higher-order finite differences

Use the quantum computer as an energy oracle. For each displaced configuration $R + l\,dR\cdot v_i$, run phase estimation to get $E(R + l\,dR\cdot v_i)$, then combine via degree-$2m$ central finite differences:

$$\frac{dE}{dR_i} = \frac{1}{dR}\sum_{l=-m}^{m} a_l^{(m)} E(R + l\,dR\cdot v_i) + \varepsilon_{\text{fd}}^{(m)}$$

where $\varepsilon_{\text{fd}}^{(m)} \sim dR^{2m+1}$ and the phase estimation error scales as $\varepsilon_{\text{PE}} \sim \lambda_H/(dR \cdot T)$. Optimising over $dR$ and $m$:

$$T_{\text{FD}} \in \tilde{O}\left(\frac{N_a^{3/2} \lambda_H c \log^{5/2}(e)}{\varepsilon}\right)$$

where $c$ and $e$ are system-dependent constants bounding derivative growth ($|d^k E / dR_i^k| \leq ec^k k^{k/2}$).

A key optimisation: the ground state can be **reused** across different displaced configurations if the perturbation is small enough ($dR \leq \gamma / (36m^2 N_a \max\|dH/dR_i\|)$, where $\gamma$ is the spectral gap), making state preparation cost additive rather than multiplicative.

### FT Algorithm 2: Overlap estimation

Block-encode the force operator $dH/dR_i$ as a unitary $U_i$ with rescaling factor $\lambda_{F_i}$. Use the overlap estimation algorithm (OEA) of Knill, Ortiz, and Somma to estimate $\langle\psi|U_i|\psi\rangle$ via phase estimation of the Szegedy walk $S = R U_i R U_i^\dagger$ where $R = I - 2|\psi\rangle\langle\psi|$.

The paper contributes several optimisations:
- Factor-of-2 speedup by using $|0\rangle\langle 0| S^{-2^{n-1}} + |1\rangle\langle 1| S^{2^{n-1}}$ instead of controlling $S^{2^n}$
- Factor-of-4 optimisation for real-valued expectation values via the identity $\langle\psi|U|\psi\rangle = \frac{1}{2}(4|\widehat{(1+\langle U\rangle)/2}|^2 - |\widehat{\langle U\rangle}|^2 - 1)$
- **State recycling:** After OEA completes, the system register has overlap $1/\sqrt{2}$ with $|\psi\rangle$ regardless of the expectation value. Repeated Hadamard tests on the reflection operator $I - 2|\psi\rangle\langle\psi|$ recover $|\psi\rangle$ at average cost $\leq 2T_R$.

Total cost:

$$T_{\text{OEA}} \in \tilde{O}\left(\varepsilon^{-1} \lambda_F N_a^{3/2} (T_F + \lambda_H \gamma^{-1} T_H)\right)$$

The $\lambda_H \gamma^{-1} T_H$ factor comes from the ground-state reflection (implemented via QSVT sign function approximation with polynomial degree $O(\lambda_H/\gamma)$).

### FT Algorithm 3: Gradient-based expectation value estimation

Apply the [[Gradient Encoding for Multi-Observable Estimation|gradient encoding trick]] from [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins et al. (2022)]]: define

$$f(\theta) = -\frac{1}{2}\text{Im}\langle\psi|\prod_{j=1}^{M} e^{-2i\theta_j O_j}|\psi\rangle + \frac{1}{2}$$

with $O_j = \frac{1}{\lambda_F} dH/dR_j$. The gradient $\partial f / \partial \theta_j |_{\theta=0} = \frac{1}{\lambda_F} dE/dR_j$, so force estimation reduces to quantum gradient estimation.

Key modification: replace all but one call to the state preparation unitary $U_\psi$ with the cheaper reflection $I - 2|\psi\rangle\langle\psi|$ by showing $G_U = U_\psi^\dagger G_U' U_\psi$ and cancelling adjacent $U_\psi U_\psi^\dagger$ pairs.

Total cost:

$$T_{\text{simultaneous}} \in \tilde{O}\left(T_H \lambda_H \gamma^{-1} N_a \lambda_F \varepsilon^{-1}\right)$$

This improves over OEA by exactly $N_a^{1/2}$ — the parallelisation speedup from estimating all $3N_a$ gradients simultaneously.

## Key results

### NISQ shot count scaling (H-chains, STO-6G, 2-norm target)

| Method | Gradient scaling | Hamiltonian scaling |
|---|---|---|
| Separate Pauli measurement | $O(N_a^{3.89}/\varepsilon^2)$ | — |
| Parallel Pauli measurement | $O(N_a^{3.69}/\varepsilon^2)$ | comparable |
| Fermionic shadow tomography | $O(N_a^{2.84}/\varepsilon^2)$ | comparable |
| Basis rotation grouping | $O(N_a^{4.72}/\varepsilon^2)$ | comparable |

### FT Toffoli complexity (gradient vector, 2-norm error $\varepsilon$)

| Algorithm | First-quantised plane waves | 2nd-quant. plane waves | H-chains (sparse) | H-chains (DF) | Water (sparse) | Water (DF) |
|---|---|---|---|---|---|---|
| Finite difference | $\tilde{O}(N^{2/3}N_a^{17/6}/\varepsilon)$ | $\tilde{O}(N^3 N_a^{3/2}/\varepsilon)$ | $\tilde{O}(N_a^{3.83}/\varepsilon)$ | $\tilde{O}(N_a^{4.44}/\varepsilon)$ | $\tilde{O}(N_a^{3.87}/\varepsilon)$ | $\tilde{O}(N_a^{4.20}/\varepsilon)$ |
| Overlap estimation | $\tilde{O}(N^{4/3}N_a^{23/6}/\varepsilon)$ | $\tilde{O}(N^3 N_a^{5/2}/\varepsilon)$ | $\tilde{O}(N_a^{4.11}/\varepsilon)$ | $\tilde{O}(N_a^{4.50}/\varepsilon)$ | $\tilde{O}(N_a^{4.30}/\varepsilon)$ | $\tilde{O}(N_a^{4.74}/\varepsilon)$ |
| Gradient-based (Huggins et al.) | $\tilde{O}(N^{4/3}N_a^{10/3}/\varepsilon)$ | $\tilde{O}(N^3 N_a^{2}/\varepsilon)$ | $\tilde{O}(N_a^{3.61}/\varepsilon)$ | $\tilde{O}(N_a^{4.00}/\varepsilon)$ | $\tilde{O}(N_a^{3.80}/\varepsilon)$ | $\tilde{O}(N_a^{4.24}/\varepsilon)$ |

### Block-encoding rescaling factors ($\lambda$)

| Method | $\lambda_H$ scaling (H-chains) | $\lambda_F$ scaling (H-chains) |
|---|---|---|
| Sparse | $O(N_H^{1.33})$ | $O(N_H^{0.28})$ |
| Double factorisation | $O(N_H^{1.94})$ | $O(N_H^{0.06})$ |

The force operator's $\lambda$ is dramatically smaller than the Hamiltonian's. This makes intuitive sense: forces are *local* (the force on a given nucleus depends mainly on nearby electrons), while the total energy is extensive.

## Comparison with prior work

| Feature | Kassal & Aspuru-Guzik (2009) | O'Brien et al. (2019) | Mitarai et al. (2020) | **This paper** |
|---|---|---|---|---|
| NISQ optimisation | No | Loose bounds | 2-RDM only | Full optimisation (IS, shadow, BRG) |
| FT algorithms | Proposed only | $N^7$–$N^{15}$ loose bounds | No | Three methods, tight asymptotic costs |
| Block encoding of $dH/dR$ | No | No | No | THC, DF, sparse, plane-wave |
| Numerical $\lambda_F$ scaling | No | No | No | H-chains, water clusters |
| Error tolerance analysis | No | No | No | 6.4 mHa/Å from MD simulation |

## Limits / caveats

1. **MD is infeasible.** Each force evaluation costs roughly the same as an energy calculation (hours on a fault-tolerant device). MD needs $10^6$–$10^9$ evaluations. No known algorithmic trick closes this gap.
2. **System-dependent constants.** The finite difference method requires bounds on higher-order energy derivatives ($c$ and $e$ in $|d^k E/dR_i^k| \leq ec^k k^{k/2}$). These are hard to estimate and could be large for some systems.
3. **Ground-state reflection dominates FT cost.** Both HF-based FT algorithms (OEA and gradient) pay $O(\lambda_H / \gamma)$ per reflection. For small-gap systems this overwhelms any savings from cheap force block-encodings.
4. **$\lambda_F$ numerics are for small systems.** The scaling $\lambda_F \in O(N_H^{0.06})$ is fitted from H-chains up to $N_H = 100$ in a minimal basis. Whether this holds for larger, more realistic systems (or larger basis sets) is open.
5. **Direct force factorisation vs. Hamiltonian differentiation.** Directly applying THC/DF to the force operator (instead of differentiating the factorised Hamiltonian) likely gives smaller $\lambda_F$ but introduces a systematic bias — the force manifold doesn't exactly match the THC-factorised energy surface.
6. **NISQ variance bounds are not tight.** The paper uses worst-case bounds on $\sigma_{i,j}$, so the comparison between methods is indicative rather than exact.

## Reusable ideas

1. [[Parallelised Importance Sampling for Vector Estimation]] — Optimise shot allocation across basis rotations to target the 2-norm of a vector of expectation values, not individual components. Saves up to $3N_a$ over separate estimation.
2. [[Force Operator Block Encoding via Hamiltonian Differentiation]] — Differentiate the factorised Hamiltonian (THC or double factorisation) to obtain a block encoding of the force operator with at most constant-factor overhead.
3. [[Ground-State Recycling in Serial Amplitude Estimation]] — After OEA, the system register always has overlap $1/\sqrt{2}$ with the ground state. Iterated Hadamard tests on the reflection operator recover $|\psi\rangle$ at average cost $\leq 2T_R$.
4. [[Finite Difference Order Optimisation for Quantum Gradient Estimation]] — Balance phase estimation error ($\sim 1/(dR \cdot T)$) against discretisation error ($\sim dR^{2m+1}$) by jointly optimising step size $dR$ and finite difference degree $m$.
5. [[FFFT Diagonalisation of Force Operators in Plane-Wave Basis]] — In plane-wave bases, all force operators are strictly one-body and mutually commuting under the [[Fermionic Fast Fourier Transform (FFFT)|FFFT]]/QFT, making them trivially simultaneous.

## References within this paper

Key cited papers with vault notes:

- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins et al. (2022)]] — The gradient-based expectation value estimation algorithm (Algorithm 3 directly builds on this)
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry et al. (2021)]] — THC block encoding of the Hamiltonian; force encoding is built by differentiating these THC tensors
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney et al. (2019)]] — Single/sparse factorisation and [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] used for force block-encodings
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes|Huggins, McClean et al. (2021)]] — Basis rotation grouping method used as a NISQ force measurement strategy
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry et al. (2021)]] — First-quantised plane-wave block encoding used for force operators
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry et al. (2019)]] — First-quantised plane-wave Hamiltonian; $\lambda_H \in O(\eta N^{2/3}/\Omega^{2/3})$
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — [[Qubitization (Quantum Walk for Spectral Encoding)|Qubitization]] framework used throughout
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al. (2018)]] — [[Double Factorization of Two-Electron Integrals|Double factorisation]] of two-electron integrals
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe et al. (2018)]] — Plane-wave dual basis Hamiltonian; [[Fermionic Fast Fourier Transform (FFFT)|FFFT]]
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean et al. (2018)]] — [[Fermionic Swap Network|Fermionic swap network]] / [[Givens Rotation Slater Determinant Preparation|Givens rotation]] circuits for basis changes
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Rubin, Lee, Babbush (2021)]] — Operator compression; noted difficulty of parallelising factorised derivative measurements

Papers cited but not in the vault:
- Knill, Ortiz, Somma (2007) — Overlap estimation algorithm (OEA)
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin and Tong (2020)]] — QSVT-based sign function for ground state reflection
- Gilyen, Arunachalam, Wiebe (2019) — Quantum gradient estimation algorithm
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan (2005)]] — Original quantum gradient estimation
- Kassal and Aspuru-Guzik (2009) — First proposal for quantum force estimation
- Helgaker and Almløf (1984) — Orbital connection theory for force operators

## Cross-links

### Related paper notes
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] — Algorithm 3 is a direct application of this paper's gradient-based method to force estimation
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC block encoding differentiated to produce force block-encodings
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — Sparse/SF methods extended to force operators
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — BRG measurement scheme used for NISQ force estimation
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — First-quantised plane-wave encoding directly used for force operators
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — Shares authors (Goings, Babbush, Rubin); CYP P450 is a pharma-relevant force estimation target
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — Analytical gradient estimation for VQE parameters (different from nuclear force gradients, but same mathematical framework)

### Related trick cards
- [[Gradient Encoding for Multi-Observable Estimation]] — The core trick behind Algorithm 3
- [[Double Factorization of Two-Electron Integrals]] — Factorisation method differentiated for force block-encodings
- [[THC Non-Orthogonal Diagonal Coulomb Representation]] — THC representation whose differentiation yields force encoding
- [[Fermionic Fast Fourier Transform (FFFT)]] — Diagonalises force operators in plane-wave basis
- [[Basis Rotation Grouping for VQE Measurement]] — NISQ measurement strategy applied to force estimation
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — Walk operator used in OEA
- [[QROAM (Space-Time Tradeoff for QROM)]] — Data loading for block-encoded force operators
- [[Phase Gradient State for Controlled Rotations]] — Used in plane-wave force SELECT implementation
- [[Coherent Alias Sampling for PREPARE]] — PREPARE circuit construction for force block encodings
- [[Pauli Expectation Value Estimation]] — Baseline NISQ measurement strategy
