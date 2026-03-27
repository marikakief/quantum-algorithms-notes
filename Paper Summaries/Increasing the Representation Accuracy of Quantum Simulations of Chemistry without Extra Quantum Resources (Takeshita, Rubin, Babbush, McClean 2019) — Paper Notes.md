> **Source:** Tyler Takeshita, Nicholas C. Rubin, Zhang Jiang, Eunseok Lee, Ryan Babbush, Jarrod R. McClean, *Increasing the representation accuracy of quantum simulations of chemistry without extra quantum resources*, arXiv:1902.10679, Phys. Rev. X **10**, 011004 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/1902.10679) · [Journal](https://doi.org/10.1103/PhysRevX.10.011004)
> **Tags:** #VQE #quantum-chemistry #active-space #NISQ #subspace-expansion #orbital-optimization #RDM #measurement

---

## The computational problem

Ground-state energy estimation for molecular electronic structure in a finite basis set. The second-quantized Hamiltonian is:

$$H = \sum_{ij} h_{ij}\, a_i^\dagger a_j + \frac{1}{2}\sum_{ijkl} h_{ijkl}\, a_i^\dagger a_j^\dagger a_k a_l$$

Near-term quantum algorithms like [[Variational Quantum Eigensolver (VQE)|VQE]] use an [[Active Space Restriction for VQE (CAS-UCC)|active-space approximation]]: partition orbitals into core ($C$), active ($A$), and virtual ($V$) sets, then solve only the active-space problem on the quantum device. This captures qualitative features (static correlation) but introduces **basis set incompleteness error** — the active-space energy can be far from the full-basis result.

The standard fix — enlarging the active space — costs more qubits and deeper circuits. This paper asks: can you recover most of the missing accuracy using only extra measurements and classical postprocessing, with zero additional quantum resources?

## What the paper does

Introduces two methods that improve on the active-space approximation without additional qubits or circuit depth:

1. **[[Virtual Quantum Subspace Expansion (VQSE)]]:** Extends the Quantum Subspace Expansion (QSE) framework by including excitations into virtual orbitals as expansion operators. The matrix elements involving virtual indices are contracted classically using Wick's theorem (the reference state has zero virtual occupancy), so only active-space RDMs need to be measured on the quantum device.

2. **Full-space orbital relaxation:** Classical MCSCF-style optimization of one-body orbital rotations over the full orbital space (active + virtual + core), using only the measured 1- and 2-RDMs from the active-space calculation.

The H₂ demonstration is striking: 4 qubits + VQSE matches the accuracy of a 20-qubit calculation in the cc-pVDZ basis. The additional cost is purely in measurements and classical postprocessing.

## The algorithm / construction

### Background: Quantum Subspace Expansion (QSE)

Given a reference state $|\Psi_\text{ref}\rangle$ (e.g., [[Variational Quantum Eigensolver (VQE)|VQE]] output), choose expansion operators $\{O_i\}$ and form the subspace basis $\{O_i |\Psi_\text{ref}\rangle\}$. Construct the projected Hamiltonian and overlap matrices:

$$H_{ij} = \langle \Psi_\text{ref}| O_i^\dagger H\, O_j |\Psi_\text{ref}\rangle, \qquad S_{ij} = \langle \Psi_\text{ref}| O_i^\dagger O_j |\Psi_\text{ref}\rangle$$

Solve the generalized eigenvalue problem $HC = SCE$ to obtain improved energies and approximate excited states.

Standard QSE for chemistry uses active-space excitation operators ($a_p^\dagger a_q$ and $a_p^\dagger a_q^\dagger a_r a_s$ with all indices in $A$). This requires measuring the active-space 4-RDM. It can refine the active-space answer but can't go beyond the active-space basis.

### [[Virtual Quantum Subspace Expansion (VQSE)]]

The key insight: expansion operators can include excitations from active to virtual orbitals — operators like $a_i^\dagger a_p$ (with $i \in A \cup V$, $p \in A$) and $a_\mu^\dagger a_q\, a_\nu^\dagger a_r$ (with $\mu, \nu \in V$; $q, r \in A$). These create excited configurations in the virtual space, expanding the effective basis beyond the active space.

The matrix elements $H_{ij}$ and $S_{ij}$ now involve virtual creation/annihilation operators acting on $|\Psi_\text{ref}\rangle$. But $|\Psi_\text{ref}\rangle$ has zero occupancy in all virtual orbitals by construction. So any virtual operator can be eliminated via Wick's theorem: virtual creation/annihilation pairs contract to Kronecker deltas, and remaining expectation values involve only active-space operators.

**Concrete example.** A matrix element between two double-excitation operators produces terms like:

$$M = \langle \Psi_\text{ref}| a_\xi a_s^\dagger a_\eta a_r^\dagger\, a_i^\dagger a_j^\dagger a_k a_l\, a_\mu^\dagger a_p\, a_\nu^\dagger a_q |\Psi_\text{ref}\rangle$$

where Greek indices ($\xi, \eta, \mu, \nu$) are virtual. Contracting the virtual pairs yields:

$$M = \Delta_{\xi\eta,\mu\nu}\, \langle a_s^\dagger a_r^\dagger a_p a_q \rangle - \delta_{\xi\nu}\, \langle a_s^\dagger a_r^\dagger a_i^\dagger a_j^\dagger a_k a_l a_p a_q \rangle\, \delta_{\eta\mu} + \ldots$$

where $\Delta_{\xi\eta,\mu\nu} = \delta_{\xi\nu}\delta_{\eta\mu} - \delta_{\xi\mu}\delta_{\eta\nu}$ and all expectation values are now over active-space indices only.

**Algorithm:**
1. Prepare $|\Psi_\text{ref}\rangle$ in the active space using [[Variational Quantum Eigensolver (VQE)|VQE]] (or any state preparation method).
2. Measure active-space RDMs up to the required order (4-RDM for singles + doubles VQSE).
3. Classically construct $H_{ij}$ and $S_{ij}$ using the measured RDMs plus full-basis one- and two-electron integrals (from classical preprocessing).
4. Solve $HC = SCE$ to get improved ground-state energy and excited states.

### Restricted active-space VQSE

For large active spaces, the 4-RDM measurement cost ($\sim N_A^8$ terms) may be prohibitive. The paper proposes splitting the active space into a "correlated core" ($A_c$) and an "active virtual" ($A_v$) subset:
- Treat $A_c$ exactly (multi-reference wavefunction on the quantum device)
- Use VQSE to include $A_v$ virtual excitations, contracting over $A_v$ indices

This reduces the RDM measurement burden to $\sim N_{A_v}^8$ for the virtual-active portion while keeping the correlated-core treatment exact.

### Full-space orbital relaxation

The orbital rotation approach is complementary to VQSE. Given measured 1- and 2-RDMs from the active-space calculation, optimize a one-body unitary $U$ acting on the full orbital space:

$$E[U] = \sum_{ij} [U h U^\dagger]_{ij}\, {}^1D_{ji} + \frac{1}{2}\sum_{ijkl} [UU h UU^\dagger]_{ijkl}\, {}^2D_{klij}$$

where ${}^1D$ and ${}^2D$ are the active-space density matrices, and $h_{ij}$, $h_{ijkl}$ are the full-basis integrals.

Parameterize $U = e^X$ with $X$ anti-Hermitian (Thouless parameterization), or use a product of Givens rotations $G_{i,b}(\theta_{i,b})$ across active↔virtual and active↔core orbital pairs. Minimize $E[U]$ classically.

Since the active-space wavefunction is variationally optimal within the active space, the only non-redundant rotations are those mixing active with core or virtual orbitals. The optimization landscape depends on the 1- and 2-RDMs only — cheap to measure compared to the 4-RDM needed for VQSE.

**Iterative MCSCF variant:** Alternate between (a) solving the active-space problem on the quantum device and (b) optimizing orbitals classically. Each round re-defines the active-space Hamiltonian by folding in the rotated integrals. This is a quantum-classical analogue of the classical CASSCF algorithm.

### Cumulant approximation for cost reduction

The 4-RDM can be decomposed via cumulants:

$${}^4D = {}^4\Delta + (\text{products of lower-order RDMs and cumulants})$$

Setting the 3- and 4-body cumulants to zero (${}^3\Delta = {}^4\Delta = 0$) expresses the 4-RDM approximately as antisymmetrized products of the 2-RDM. This is the "cumulant truncation" approximation, reducing the measurement overhead from $\sim N_A^8$ to $\sim N_A^4$ at the cost of introducing approximation error. The paper notes this is systematically improvable by including higher-order cumulants.

## Key results

**H₂ in STO-3G (4 qubits, active space = full space):**
- Standard QSE and VQSE recover essentially exact energies (trivially, since active = full in this case).

**H₂ in cc-pVDZ (4 active qubits out of 20 total):**
- Active-space-only: significant basis error relative to full 20-qubit result.
- VQSE (singles + doubles): recovers most of the missing correlation, approaching the full-basis accuracy.
- 4 qubits + VQSE matches the 20-qubit result to high precision.

**Orbital relaxation for H₂:**
- Single-step orbital relaxation using COBYLA-optimized Givens rotation angles lowers the energy relative to the fixed active-space result.
- Combined with VQSE, provides further improvement.

The paper does not provide general asymptotic complexity bounds beyond the measurement scaling discussed above. The results are proof-of-concept on a small system.

## Comparison with prior work

| Method | Extra qubits | Extra depth | Extra measurements | Classical post | Dynamic correlation? |
|---|---|---|---|---|---|
| Larger active space | Yes ($N_V$ more) | Yes | Proportional | Minimal | Within active space only |
| [[Active Space Restriction for VQE (CAS-UCC)\|CAS-UCC]] (Romero et al.) | None | None | None | None | No |
| NEVPT2 / CASPT2 (classical) | — | — | — | $O(N^5)$–$O(N^6)$ | Yes (perturbative) |
| Standard QSE (Colless et al.) | None | None | Up to 4-RDM in $A$ | $O(N_A^8)$ | No (stays in $A$) |
| **VQSE (this paper)** | **None** | **None** | Up to 4-RDM in $A$ | $O(N_A^8 + N_V \cdot N_A^4)$ | **Yes** |
| **Orbital relaxation (this paper)** | **None** | **None** | 1- and 2-RDM only | $O(N^2)$ optimization | **Partially** |

## Limits / caveats

- **4-RDM measurement scaling:** Full VQSE with singles and doubles needs $\sim N_A^8$ distinct measurement terms. For $N_A \gtrsim 20$, this becomes the practical bottleneck. Cumulant truncation helps but introduces systematic error whose magnitude is hard to predict a priori.
- **H₂ is tiny:** The numerical demonstrations are on H₂ only. Whether the accuracy gains transfer to strongly correlated systems (transition metals, bond-breaking in polyatomics) is an open question. My assessment: for systems where the active space captures static correlation well but misses dynamic correlation with virtual orbitals, this should work. For systems where the active space itself is insufficient, VQSE won't save you.
- **No error analysis under noise:** The paper assumes noiseless RDM measurements. On real NISQ hardware, RDM estimation is noisy, and errors propagate through the Wick contractions and the generalized eigenvalue solve. The [[2-RDM Physicality Restoration by PSD Projection|RDM projection techniques]] from Rubin, Babbush, and McClean (2018) would be a natural companion here, but the combination isn't explored.
- **Orbital relaxation is a local optimization:** The MCSCF-style orbital optimization can converge to local minima, particularly for near-degenerate orbital rotations. This is a known issue in classical CASSCF.
- **Relationship to classical methods:** VQSE is essentially the quantum analogue of internally contracted MRCI (ic-MRCI). The advantage over classical MRCI is that the reference wavefunction can represent exponentially many determinants (prepared on the quantum device), whereas classical MRCI is limited by the classical cost of the reference. For small active spaces where classical FCI is feasible, there's no advantage.

## Reusable ideas

1. [[Virtual Quantum Subspace Expansion (VQSE)]] — Extend QSE beyond the active space by including virtual-orbital excitations, then contract virtual indices classically via Wick's theorem. Only active-space RDMs are needed from the quantum device.

2. [[Orbital Relaxation via RDM Postprocessing]] — Use measured 1- and 2-RDMs to classically optimize one-body orbital rotations over the full orbital space. A cheap postprocessing step that can lower the energy without any additional quantum computation.

3. [[Cumulant Truncation for RDM Measurement Reduction]] — Approximate higher-order RDMs (3-RDM, 4-RDM) as antisymmetrized products of lower-order RDMs by setting high-order cumulants to zero. Trades measurement overhead for approximation error.

## References within this paper

- Colless et al. (2018), Phys. Rev. X 8, 011021 — Original quantum subspace expansion (QSE) proposal. The paper extends this by adding virtual excitations.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes|Romero, Babbush et al. (2018)]] — Active-space restriction for VQE (CAS-UCC). VQSE addresses the basis incompleteness that CAS-UCC leaves behind.
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes|Rubin, Babbush, McClean (2018)]] — RDM measurement techniques and $n$-representability constraints. The RDM machinery here is directly relevant to VQSE's measurement requirements.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — First scalable VQE demonstration. VQSE builds on the VQE framework demonstrated there.
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman, Mejuto-Zaera, Babbush et al. (2018)]] — State preparation for quantum chemistry. VQSE uses the same active-space reference state that Tubman et al. analyze for overlap quality.
- Roos, Taylor, Siegbahn (1980); Werner, Knowles (1985) — Classical CASSCF and MRCI methods. VQSE is the quantum analogue of internally contracted MRCI.
- Kutzelnigg, Mukherjee (1997) — Cumulant decomposition of RDMs. Used here for measurement reduction.
- McClean, Kimchi-Schwartz et al. (2017) — Earlier QSE work for error mitigation. VQSE repurposes the QSE framework for basis extension rather than error correction.
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. (2014)]] — Original [[Variational Quantum Eigensolver (VQE)|VQE]] proposal.

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — Original VQE proposal; VQSE takes the VQE reference state and extends its accuracy via classical postprocessing alone
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — RDM measurement and $n$-representability; directly relevant measurement infrastructure
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — CAS-UCC active-space restriction; VQSE addresses the residual basis error
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — VQE framework that VQSE builds on
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — Active-space state preparation quality
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — Fault-tolerant approach to full-basis chemistry; VQSE offers a NISQ alternative for recovering basis completeness
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — Alternative virtual correlation approach via [[Virtual Correlation via Matchgate Contraction|matchgate contraction]] that avoids the 3/4-RDM measurement overhead of VQSE
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]] — Companion paper (same month, same group) applying the QSE framework to error mitigation rather than basis extension
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes|Huggins, McClean, Rubin, Babbush et al 2021]]

### Trick cards
- [[Variational Quantum Eigensolver (VQE)]] — Produces the reference state for VQSE
- [[Active Space Restriction for VQE (CAS-UCC)]] — The approximation that VQSE corrects
- [[Unitary Coupled Cluster (UCC) Ansatz]] — Common ansatz for the active-space reference
- [[2-RDM Physicality Restoration by PSD Projection]] — Natural companion for noisy RDM inputs to VQSE
- [[Variance Reduction via N-Representability Constraints (LP Method)]] — Reduces measurement cost for the RDMs that VQSE requires
- [[Virtual Quantum Subspace Expansion (VQSE)]] — Main trick from this paper
- [[Orbital Relaxation via RDM Postprocessing]] — Orbital optimization trick from this paper
- [[Cumulant Truncation for RDM Measurement Reduction]] — Measurement reduction trick from this paper
