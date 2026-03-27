> **Source:** William J. Huggins, Bryan A. O'Gorman, Nicholas C. Rubin, David R. Reichman, Ryan Babbush, Joonho Lee, *Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer*, arXiv:2106.16235, Nature **603**, 416–420 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2106.16235) · [Journal](https://doi.org/10.1038/s41586-021-04351-z)
> **Tags:** #QMC #AFQMC #hybrid-algorithm #NISQ #quantum-chemistry #shadow-tomography #sign-problem #trial-wavefunction

---

## The computational problem

Ground-state energy estimation for many-electron systems. The formal problem: given a second-quantized electronic Hamiltonian $\hat{H}$ with $N$ spin-orbitals and $\eta$ electrons, estimate the ground-state energy $E_0$ to within some target accuracy (typically chemical accuracy, 1 kcal/mol).

The classical approach via projector quantum Monte Carlo (QMC) stochastically implements imaginary-time evolution $|\Psi_0\rangle \propto \lim_{\tau \to \infty} e^{-\tau \hat{H}} |\Phi_0\rangle$. This is formally exact but hits the **fermionic sign problem**: walker weights alternate in sign, causing exponentially growing variance in the energy estimator. Controlling the sign problem requires imposing constraints (fixed-node, phaseless) using a **trial wavefunction** $|\Psi_T\rangle$, which introduces bias. The bias is entirely determined by the quality of $|\Psi_T\rangle$.

The question this paper addresses: can a quantum computer provide trial wavefunctions that reduce the constraint bias in QMC beyond what's achievable classically?

## What the paper does

Proposes **QC-QMC** (quantum-classical hybrid QMC): use a quantum computer to prepare a trial wavefunction $|\Psi_T\rangle$ that would be classically intractable, then use it to constrain an otherwise classical AFQMC calculation. The quantum device never represents the ground state directly — it only provides the constraint. The heavy lifting (imaginary-time evolution, energy estimation) stays on the classical computer.

The concrete implementation, **QC-AFQMC**, uses shadow tomography to characterize the quantum trial state offline, eliminating iterative quantum-classical feedback during the QMC run. Experiments on Google's Sycamore processor (8, 12, and 16 qubits) unbias AFQMC calculations on systems with up to 120 orbitals, achieving accuracy competitive with CCSD(T) — the classical "gold standard" — on strongly correlated systems where both CCSD(T) and standard AFQMC fail.

This is a genuinely different hybrid quantum-classical paradigm from [[Variational Quantum Eigensolver (VQE)|VQE]]. VQE tries to represent the ground state on the quantum device and extract the energy directly. QC-QMC uses the quantum device as an oracle for a classical algorithm that does the actual energy computation. The noise tolerance follows naturally: the quantum device only needs to provide overlap *ratios*, not absolute energies, and these ratios are inherently resilient to depolarizing noise.

## The algorithm / construction

### AFQMC background

In auxiliary-field QMC (AFQMC), the imaginary-time propagation is represented stochastically via Hubbard-Stratonovich transformation. Walker wavefunctions $|\phi_i(\tau)\rangle$ are single Slater determinants throughout the evolution. The energy estimator is the "mixed" estimator:

$$E(\tau) = \frac{\langle \Psi_T | \hat{H} | \Psi(\tau) \rangle}{\langle \Psi_T | \Psi(\tau) \rangle} = \frac{\sum_i w_i E^{(i)}(\tau)}{\sum_i w_i}$$

where $E^{(i)}(\tau) = \langle \Psi_T | \hat{H} | \phi_i(\tau) \rangle / \langle \Psi_T | \phi_i(\tau) \rangle$ is the local energy for walker $i$.

The **phaseless constraint** controls the sign problem by updating walker weights using:

$$|S_i(\tau)| \times \max(0, \cos \theta_i(\tau))$$

where $S_i(\tau) = \langle \Psi_T | \phi_i(\tau + \Delta\tau) \rangle / \langle \Psi_T | \phi_i(\tau) \rangle$. Walkers with negative real overlap relative to $|\Psi_T\rangle$ are killed. This introduces bias unless $|\Psi_T\rangle$ is exact.

### QC-QMC: the quantum ingredient

The quantum computer's role is to provide $|\Psi_T\rangle$ from a class that's classically intractable to evaluate overlaps for. The quantities needed by AFQMC are:

1. **Overlap ratio** $S_i(\tau) = \langle \Psi_T | \phi_i(\tau+\Delta\tau) \rangle / \langle \Psi_T | \phi_i(\tau) \rangle$ (for the phaseless constraint)
2. **Local energy** $E^{(i)}(\tau)$ (for the energy estimator)

The local energy can be decomposed via Slater-Condon rules into $O(N^4)$ overlaps of $|\Psi_T\rangle$ with single and double excitations of the walker:

$$\langle \Psi_T | \hat{H} | \phi_i \rangle = \sum_{pr} \langle \Psi_T | \phi^r_p \rangle \langle \phi^r_p | \hat{H} | \phi_i \rangle + \sum_{pqrs} \langle \Psi_T | \phi^{rs}_{pq} \rangle \langle \phi^{rs}_{pq} | \hat{H} | \phi_i \rangle$$

So the entire algorithm reduces to estimating overlaps $\langle \Psi_T | \phi \rangle$ for various Slater determinants $|\phi\rangle$.

### Shadow tomography for offline overlap estimation

Rather than iteratively querying the quantum device (Hadamard test for each walker at each time step), the paper uses [[Shadow Tomography for QMC Overlap Estimation|classical shadow tomography]] to characterize $|\Psi_T\rangle$ offline.

Prepare the state $|\tau\rangle = (|0\rangle + |\Psi_T\rangle)/\sqrt{2}$. Then the overlap is:

$$\langle \phi | \Psi_T \rangle = 2\langle \phi | \tau \rangle \langle \tau | 0 \rangle = 2\,\text{Tr}[|\tau\rangle\langle\tau| \cdot |0\rangle\langle\phi|]$$

Using Huang et al.'s shadow tomography protocol with $N$-qubit Clifford measurements, the number of measurement repetitions to estimate $M$ overlaps to additive error $\varepsilon$ with failure probability $\delta$ is:

$$R = O\!\left(\frac{\log M - \log \delta}{\varepsilon^2}\right)$$

This scales only logarithmically in the number of walkers — a huge advantage over per-walker Hadamard tests.

### Trial wavefunctions used

The experiments use **perfect pairing (PP)** states — a restricted coupled-cluster form:

$$|\Psi_\text{PP}\rangle = e^{\hat{T}_\text{PP}} e^{\hat{\kappa}} |\psi_0\rangle$$

where $\hat{T}_\text{PP} = \sum_i t_i \hat{a}^\dagger_{i^*\uparrow} \hat{a}_{i\uparrow} \hat{a}^\dagger_{i^*\downarrow} \hat{a}_{i\downarrow}$ pairs each occupied orbital with one virtual orbital, and $\hat{\kappa}$ is a single-particle rotation. For H$_4$, additional hardware-efficient layers (density-density and hopping terms) are added on top of PP.

No classical algorithm is known for computing $\langle \phi | \Psi_\text{PP} \rangle$ exactly in polynomial time (where $|\phi\rangle$ is an arbitrary Slater determinant), though an efficient classical algorithm exists for *approximating* this overlap to additive error. So the PP experiments do not demonstrate quantum advantage in the overlap estimation itself — but the framework extends to trial wavefunctions (UCC, hardware-efficient, 2D-MERA) where no such classical approximation is known.

### Virtual correlation energy

A key practical feature: the quantum trial acts on an active space of $N_A$ qubits, but the AFQMC runs over the full orbital space ($N$ orbitals). The overlap between the full trial $|\Psi_T\rangle = |\psi_T\rangle \otimes |\psi_c\rangle \otimes |0_v\rangle$ and a walker $|\phi\rangle$ reduces to:

$$\langle \phi | \Psi_T \rangle = \text{const} \times \langle \tilde{\phi} | \psi_T \rangle$$

where $|\tilde{\phi}\rangle$ is a Slater determinant in the active space obtained by [[Virtual Correlation via Matchgate Contraction|contracting out the frozen core and virtual orbitals]] using matchgate tensor properties. This requires zero additional quantum resources — the contraction is classical.

This is similar in spirit to [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes|VQSE (Takeshita et al. 2019)]], which also recovers virtual correlation classically. But VQSE requires measuring 3- and 4-RDMs in the active space (measurement overhead scaling as $N_A^8$ before cumulant truncation). QC-AFQMC gets the virtual correlation for free.

### Noise resilience

The overlap *ratios* that AFQMC actually uses are inherently [[Noise-Resilient Overlap Ratios in QC-QMC|resilient to noise]]. Under global depolarizing noise $\rho \mapsto (1-p)\rho + pI$, the noise factor cancels exactly in the ratio:

$$\frac{\langle \phi_1 | \rho' | 0 \rangle}{\langle \phi_2 | \rho' | 0 \rangle} = \frac{(1-p)\langle \phi_1 | \rho | 0 \rangle}{(1-p)\langle \phi_2 | \rho | 0 \rangle} = \frac{\langle \phi_1 | \rho | 0 \rangle}{\langle \phi_2 | \rho | 0 \rangle}$$

because $\langle \phi_i | 0 \rangle = 0$ (the trial has nonzero particle number). This extends to the robust shadow tomography framework (gate-independent, time-stationary, Markovian noise). Partitioned shadow tomography (splitting spin sectors) preserves this property as long as each spin sector has nonzero particle number.

## Key results

### H$_4$ (8 qubits, 4 and 120 orbitals)

Square H$_4$ at 1.23 Å — a strongly correlated system where both CCSD(T) and standard AFQMC fail.

| Method | Atomization energy (kcal/mol, STO-3G) | Atomization energy (kcal/mol, cc-pVQZ) |
|---|---|---|
| Exact | 64.7 | 70.5 |
| CCSD(T) | 59.6 | 71.9 |
| AFQMC (classical trial) | 62.9 | 68.6 |
| Quantum trial (variational) | 55.2 | 37.4 |
| **QC-AFQMC** | **64.3** | **69.7** |

The quantum trial energy itself is terrible (33 kcal/mol error in cc-pVQZ!) due to hardware noise. But QC-AFQMC achieves chemical accuracy in both bases. The trial doesn't need to be energetically good — it needs to have the right nodal/phase structure.

### N$_2$ (12 qubits, 60 orbitals)

Bond-stretching from 1.0 to 2.25 Å in cc-pVTZ. CCSD(T) error reaches 14 kcal/mol at stretched geometries; AFQMC error reaches 8 kcal/mol. QC-AFQMC error stays within $\pm 2$ kcal/mol across the full curve, even with a simple PP trial (no hardware-efficient layers).

### Diamond (16 qubits, 26 orbitals)

Minimal unit cell, DZVP-GTH basis. The largest quantum chemistry simulation on a quantum computer at time of publication. QC-AFQMC substantially improves over classical AFQMC across the full range of lattice constants, with accuracy comparable to CCSD(T).

### Measurement scaling

Shadow tomography with $N$-qubit Cliffords: $R = O(\log M / \varepsilon^2)$ measurements for $M$ overlap queries. Partitioned shadow tomography (splitting into spin sectors) reduces circuit depth at the cost of more measurements, but performed well in practice. The shadow tomography circuits require depth at most $2n+2$ in CZ gates using the Bravyi-Maslov H-free decomposition — much better than the $9n$ depth for a general Clifford.

## Comparison with prior work

| Approach | Quantum role | Noise tolerance | Classical overhead | Scalability bottleneck |
|---|---|---|---|---|
| [[Variational Quantum Eigensolver (VQE)\|VQE]] | Represents ground state, evaluates energy | Moderate (variational principle helps) | Optimization, $O(N^4/\varepsilon^2)$ measurements | Barren plateaus, circuit depth, shot noise |
| QPE / [[Iterative Phase Estimation (Kitaev)\|Kitaev PE]] | Full ground-state projection | Low (fault tolerance needed) | Minimal | Coherent circuit depth, error correction |
| **QC-QMC** | Provides trial wavefunction constraint | **High** (overlap ratios cancel noise) | Shadow tomography classical post-processing | Overlap decay with system size |

### Scale comparison with prior quantum chemistry experiments

| Experiment | Qubits | Two-qubit gates | Reference |
|---|---|---|---|
| Kandala et al. (BeH$_2$) | 6 | 5 | Nature 549 (2017) |
| Nam et al. (H$_2$O) | 5 | 6 | npj QI 6 (2020) |
| Google AI (Hartree-Fock) | 12 | 72 | Science 369 (2020) |
| **QC-AFQMC (diamond)** | **16** | **160** | **This work** |

## Limits / caveats

1. **Exponential classical post-processing (now resolved):** The Clifford shadow tomography protocol used here requires enumerating all computational basis states to compute $\langle \phi | U_k^\dagger | b_k \rangle$ when $|\phi\rangle$ is a Slater determinant, scaling exponentially in system size. This bottleneck was removed by [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|Wan, Huggins, Lee, and Babbush (2022)]], who replaced Clifford circuits with [[Matchgate 3-Design for Classical Shadows|matchgate circuits]] (fermionic Gaussian unitaries). The resulting [[Pfaffian Shadow Estimation for Fermionic Observables|matchgate shadow]] post-processing runs in $O(n^4)$ per sample, with sublinear-in-$n$ variance — making QC-AFQMC genuinely scalable on the classical side.

2. **Overlap decay:** The overlap $\langle \Psi_T | \phi \rangle$ decays exponentially with system size (e.g., $\sim 10^{-5}$ at 16 atoms, $\sim 10^{-38}$ at 128 atoms). AFQMC needs overlap *ratios* to fixed relative precision, so this formally requires exponentially many measurements. The virtual correlation technique helps by keeping the active space small, but this is a fundamental scaling concern.

3. **No demonstrated quantum advantage:** The PP trial used in experiments has an efficient classical approximation for overlap estimation. Quantum advantage requires trial wavefunctions (UCC, hardware-efficient with many layers, 2D-MERA) where no such classical algorithm exists. The paper is honest about this — it's a proof of concept for the framework.

4. **Limited by trial quality:** QC-AFQMC can only be as good as its trial wavefunction's phase/nodal structure. A noisy trial with bad phase structure will still give biased results. The noise resilience is for *amplitude* noise, not structural errors in the trial.

5. **Follow-up controversy:** Mazzola and Carleo (arXiv:2205.09820) subsequently showed exponential challenges can arise for QC-QMC in certain regimes. Huggins et al. responded (arXiv:2207.13776) acknowledging these challenges while arguing they don't invalidate the approach for practical systems with finite correlation length.

## Reusable ideas

1. [[Shadow Tomography for QMC Overlap Estimation]] — Use classical shadows to estimate trial-walker overlaps offline, decoupling the quantum and classical computers during the QMC run. Logarithmic scaling in the number of overlap queries.

2. [[Virtual Correlation via Matchgate Contraction]] — Reduce a full-space overlap to an active-space overlap by exploiting matchgate tensor structure of Slater determinants. Gets virtual correlation energy for free (no extra qubits, no extra measurements). Applies whenever the trial factorizes as active $\otimes$ core $\otimes$ vacuum.

3. [[Noise-Resilient Overlap Ratios in QC-QMC]] — Overlap ratios cancel depolarizing noise exactly because the vacuum component $\langle \phi | 0 \rangle = 0$ for any state with nonzero particle number. Extends to robust shadow tomography under gate-independent Markovian noise.

## References within this paper

Key citations and their role:

- **Zhang & Krakauer (2003)** — Introduces the phaseless approximation for AFQMC. The constraint mechanism that QC-QMC aims to improve.
- **[[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. (2014)]]** — Original [[Variational Quantum Eigensolver (VQE)|VQE]] proposal. QC-QMC is positioned as a fundamentally different hybrid paradigm.
- **Huang, Kueng, Preskill (2020)** — Classical shadow tomography framework. The measurement protocol underlying the experiments.
- **[[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|Aaronson (2018)]]** — Original shadow tomography concept (different protocol).
- **Troyer & Wiese (2005)** — QMA-completeness of the sign problem. Establishes the fundamental hardness.
- **Goddard et al. (1973); Cullen (1996)** — Perfect pairing (PP) wavefunction theory. The trial ansatz used in all experiments.
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes|Takeshita, Rubin, Babbush, McClean (2020)]] — Prior virtual correlation strategy within VQE (VQSE). QC-AFQMC's virtual correlation is more efficient (no RDM overhead).
- **Zhao, Rubin, Miyake (2020)** — Fermionic shadow tomography. Potential solution to the exponential post-processing bottleneck.
- **Bravyi & Maslov (2020)** — Clifford circuit decomposition used to optimize shadow tomography circuit depth.
- **Chen et al. (2021, robust shadows)** — Robust shadow tomography. The noise analysis builds on this.
- **Arute et al. (2019, Sycamore)** — The quantum hardware used for experiments.
- **Motta & Zhang (2018)** — AFQMC review. Background for the classical QMC algorithm.

## Cross-links

### Paper notes in the vault

- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — Resolves the exponential classical post-processing bottleneck in QC-AFQMC by replacing Clifford shadows with matchgate shadows; $O(n^4)$ per sample, sublinear variance.
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] — Virtual correlation strategy comparison. VQSE requires 3/4-RDM measurements; QC-AFQMC gets it for free via matchgate contraction.
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — Rubin is a coauthor on both. The $n$-representability constraints could potentially improve QC-AFQMC trial wavefunctions or post-processing.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — Prior VQE hardware experiments that QC-AFQMC's scale surpasses.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — UCC mentioned as a candidate trial wavefunction for future QC-QMC work.
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — Overlap decay problem is central to both papers. Tubman et al. study initial-state overlap for QPE; QC-QMC faces the same concern for trial-walker overlaps.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — [[Givens Rotation Slater Determinant Preparation|Givens rotations]] used in the trial state preparation circuits.
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Same lead author (Huggins); the [[Basis Rotation Grouping for VQE Measurement|basis rotation grouping]] and [[Number-Basis Postselection for Error Mitigation|number-basis postselection]] ideas from that paper inform the noise-resilient measurement approach used here
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — Shares authors (Babbush, McClean, Neven); uses the same classical shadow formalism (Huang, Kueng, Preskill 2020) for [[Projected Quantum Kernel via Local Observables|projected quantum kernels]]
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush 2021]]
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — Argues against generic EQA for ground-state chemistry; QC-QMC's value may lie precisely in polynomial advantage (better trial wavefunctions for AFQMC) rather than exponential speedup
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]] — Another Sycamore hardware experiment on correlated chemistry; uses direct QITE simulation rather than QC-QMC's trial-wavefunction paradigm; complementary demonstration of near-term hardware limits
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] — same lead author (Huggins); gradient encoding approach provides the fault-tolerant counterpart to the shadow tomography offline measurement used here, both addressing multi-observable estimation
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — applies the QC-AFQMC framework to a pharmaceutically relevant system; CYP P450 is the drug-metabolism target that tests scalability of the hybrid paradigm established here
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]] — two-copy fermionic shadow tomography that could reduce the shadow sample cost in QC-AFQMC beyond what matchgate shadows achieve; relevant to the scalability of the offline overlap estimation step

### Trick cards in the vault

- [[Variational Quantum Eigensolver (VQE)]] — The main competing NISQ paradigm. QC-QMC avoids VQE's optimization difficulties and measurement overhead.
- [[Unitary Coupled Cluster (UCC) Ansatz]] — Candidate trial wavefunction for future QC-QMC experiments.
- [[Givens Rotation Slater Determinant Preparation]] — Used in the experimental circuit construction.
- [[Pauli Expectation Value Estimation]] — Related measurement technique; shadow tomography is an alternative approach.
- [[ASCI Overlap Estimation for QPE Initial State]] — Different approach to the overlap estimation problem.
- [[Virtual Quantum Subspace Expansion (VQSE)]] — Alternative virtual correlation technique with higher measurement overhead.
- [[Shadow Tomography for QMC Overlap Estimation]] — New trick card from this paper.
- [[Virtual Correlation via Matchgate Contraction]] — New trick card from this paper.
- [[Noise-Resilient Overlap Ratios in QC-QMC]] — New trick card from this paper.
