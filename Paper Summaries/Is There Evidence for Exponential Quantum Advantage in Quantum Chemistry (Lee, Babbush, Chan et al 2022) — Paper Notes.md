> **Source:** Seunghoon Lee, Joonho Lee, Huanchen Zhai, Yu Tong, Alexander M. Dalzell, Ashutosh Kumar, Phillip Helms, Johnnie Gray, Zhi-Hao Cui, Wen-Yuan Liu, Michael Kastoryano, Ryan Babbush, John Preskill, David R. Reichman, Earl T. Campbell, Edward F. Valeev, Lin Lin, Garnet Kin-Lic Chan, *Is there evidence for exponential quantum advantage in quantum chemistry?*, arXiv:2208.02199 (2022)
> **Published as:** *Evaluating the evidence for exponential quantum advantage in ground-state quantum chemistry*, Nature Communications **14**, 1952 (2023)
> **Links:** [arXiv](https://arxiv.org/abs/2208.02199) · [Nature Communications](https://www.nature.com/articles/s41467-023-37587-6)
> **Tags:** #quantum-chemistry #quantum-advantage #complexity #state-preparation #classical-heuristics #DMRG #coupled-cluster #tensor-networks #FeMoCo #orthogonality-catastrophe

---

## The computational problem

Compute the ground-state energy $E$ of the electronic Schrödinger Hamiltonian for a chemical system in a basis of size $L$, where increasing $L$ corresponds to increasing physical system size (number of atoms) with basis size proportional to system size. The relevant error metric is either $\varepsilon$ (absolute energy error) or $\bar{\varepsilon} = \varepsilon/L$ (energy density error). The question: does this task admit exponential quantum advantage (EQA) — i.e., is there a large class of "generic" chemical problems where quantum algorithms run in $\mathrm{poly}(L)$ time but the best classical heuristics require $\exp(L)$?

---

## What the paper does

Gathers numerical and theoretical evidence bearing on the EQA hypothesis for ground-state quantum chemistry. The answer is negative: they find no evidence for generic exponential quantum speedup. Quantum state preparation faces the same obstacles that make problems hard classically, and classical heuristics empirically scale polynomially for the problems tested. The paper doesn't prove EQA is impossible — it can't, because "generic chemistry" is not a precise complexity class — but it makes a well-supported case that EQA should not be the default assumption.

This is one of the most important papers in the quantum algorithms for chemistry space, because it directly challenges the narrative that chemistry is quantum computing's "killer app" in the exponential-speedup sense. The authorship — spanning Caltech, Google Quantum AI, AWS, Columbia, Berkeley, Riverlane — gives it unusual weight. My assessment: the argument is convincing for ground-state energy estimation. The open question is whether other tasks (dynamics, excited states, Green's functions) tell a different story.

---

## The argument structure

The paper's logic has two prongs:

### Prong 1: Quantum state preparation is not obviously easy

The cost of [[Qubitization (Quantum Walk for Spectral Encoding)|QPE]]-based ground-state energy estimation is:

$$\mathrm{poly}(1/S) \cdot [\mathrm{poly}(L) \cdot \mathrm{poly}(1/\varepsilon) + C]$$

where $S = |\langle \Phi | \Psi_0 \rangle|$ is the overlap between the prepared initial state $|\Phi\rangle$ and the true ground state $|\Psi_0\rangle$, and $C$ is the state-preparation cost. The $\mathrm{poly}(L) \cdot \mathrm{poly}(1/\varepsilon)$ piece is the phase estimation circuit (well-understood, polyomial in $L$). The $\mathrm{poly}(1/S)$ piece — specifically $1/S^2$ for standard QPE — is the repetition overhead.

Two state preparation strategies are examined:

**Ansatz state preparation:** Prepare a classically specified state (e.g., Hartree-Fock). For Fe-S clusters ranging from 2 to 8 metal centres, the weight $S^2$ of the best single Slater determinant decays exponentially with system size — reaching $\sim 10^{-7}$ for FeMo-co. Configuration state functions (spin eigenstates) improve things but still show exponential decay. This is the [[ASCI Overlap Estimation for QPE Initial State|orthogonality catastrophe]] in action. Improving the ansatz to get $1/\mathrm{poly}(L)$ overlap requires a classical ansatz powerful enough that it can also solve for the energy to the target precision — undermining the claimed quantum advantage.

**[[Stroboscopic Adiabatic Walk|Adiabatic state preparation]] (ASP):** Slowly evolve from the ground state of a solvable Hamiltonian $H(0)$ to the target Hamiltonian $H(1)$. For a 24-qubit [2Fe-2S] model, $T_{\mathrm{ASP}}$ varies over 8 orders of magnitude depending on the choice of $H(0)$, and correlates as $T_{\mathrm{ASP}} \sim \mathrm{poly}(1/|\langle \Upsilon_0 | \Psi_0 \rangle|)$ — reminiscent of unstructured search. The lowest-energy mean-field Hamiltonian gives $T_{\mathrm{ASP}} > T_{\mathrm{QPE}}$. Getting $T_{\mathrm{ASP}} < T_{\mathrm{QPE}}$ requires starting from an interacting Hamiltonian that already includes most of the interactions (20 out of 24 qubits). Finding a good $H(0)$ is itself a heuristic problem — and if classical heuristics solve it efficiently, why can't they also solve the original problem?

### Prong 2: Classical heuristics don't appear to be exponentially hard

**Single-reference systems (coupled cluster):** For N₂, the error of coupled cluster converges as $\mathrm{poly}(1/\varepsilon)$ with increasing excitation level (CCSD, CCSDT, CCSDTQ, ...). Local coupled cluster (DLPNO-CCSD(T)) scales nearly linearly in $L$ for gapped systems — demonstrated on n-alkanes up to C₁₂₀H₂₄₂ and a 565-atom photosystem II fragment. Conjectured cost: $O(L) \cdot \mathrm{poly}(1/\bar{\varepsilon})$.

**Strongly correlated systems (tensor networks):** PEPS calculations on the 3D Heisenberg model (up to 1000 sites) show cost close to $O(L)$ for constant $\bar{\varepsilon}$. For the 2D Hubbard model at the notoriously difficult 1/8-doping point, $\bar{\varepsilon} \sim 1/\mathrm{poly}(D)$ with weak $L$-dependence. Conjectured cost: $\mathrm{poly}(L) \cdot \mathrm{poly}(D) \cdot \mathrm{poly}(1/\bar{\varepsilon})$.

**Embedding methods:** DMET on hydrogen cubes up to $10 \times 10 \times 10$ (1000 atoms, minimal basis) shows poly$(L)$ time per atom.

---

## Key results

No formal theorems are proved. The main results are empirical observations supported by theoretical framing:

1. **Ansatz overlap decay (Fig. 1):** For Fe-S clusters, both the best single-determinant weight and best CSF weight decrease exponentially with the number of metal centres. FeMo-co (8 metals): $S^2_{\mathrm{det}} \sim 10^{-7}$, $S^2_{\mathrm{CSF}} \sim 10^{-4}$.

2. **ASP cost–overlap correlation (Fig. 2):** $T_{\mathrm{ASP}}^{\mathrm{est}} \sim \mathrm{poly}(1/|\langle \Upsilon_0 | \Psi_0 \rangle|)$ and $1/\min_s \Delta(s) \sim \mathrm{poly}(1/|\langle \Upsilon_0 | \Psi_0 \rangle|^2)$ across both mean-field and interacting initial Hamiltonians.

3. **Classical CC scaling (Fig. 3):** Error of CC hierarchy consistent with $\mathrm{poly}(1/\varepsilon)$; local CC timing consistent with $O(L)$ for alkanes.

4. **Classical tensor network scaling (Fig. 4):** PEPS on 3D Heisenberg at constant $\bar{\varepsilon}$ with cost close to $O(L)$; PEPS on 2D Hubbard with $\bar{\varepsilon} \sim 1/\mathrm{poly}(D)$.

5. **DMRG bond dimension scaling (Fig. S3):** The MPS bond dimension needed for $\bar{\varepsilon} = 10^{-3}$ Hartree/metal in Fe-S clusters shows sub-volume growth between 4 and 8 metal centres, hinting at area-law behaviour.

6. **Energy convergence is superpolynomial in cost (Fig. S9):** For DMRG on Fe-S clusters, $\log(1/\bar{\varepsilon}) \sim (\log T)^2$, corresponding to superpolynomial but *not* exponential convergence.

---

## Comparison with prior work

| Claim | Prior conventional wisdom | This paper's finding |
|---|---|---|
| QPE gives exponential speedup for chemistry | Often stated or implied (e.g., Reiher et al. 2017) | QPE circuit is $\mathrm{poly}(L)$, but state preparation cost must be accounted for; may eliminate exponential gap |
| FeMoCo is a poster-child for quantum advantage | Widely assumed since Reiher et al. 2017 | HF overlap $\sim 10^{-7}$ for FeMoCo; state preparation overhead is massive; DMRG can approach the answer classically |
| Classical methods fail for strongly correlated systems | Common assumption | Tensor networks, QMC, embedding methods all show poly-cost scaling in tested domains |
| Adiabatic state preparation solves the overlap problem | Hoped for in several proposals | ASP cost correlates with overlap; finding a good starting Hamiltonian is itself hard |

Key prior work this paper builds on or challenges:
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman et al. (2018)]]: showed HF overlap $\geq 0.75$ for *most* systems — but this paper's Fe-S data shows the overlap catastrophe is real for the very systems quantum computing was supposed to help with.
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry, Babbush et al. (2021)]] and [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney et al. (2019)]]: progressively lower resource estimates for FeMoCo, but this paper questions whether FeMoCo (or similar problems) actually needs a quantum computer.
- Gharibian & Le Gall (2021): showed that ground-state energy estimation to inverse-polynomial precision is BQP-hard (assuming good initial state), establishing a formal quantum-classical separation — but the relevant Hamiltonians may not arise in generic chemistry.

---

## Limits / caveats

- The paper restricts attention to **ground-state energy estimation**. Other tasks — real-time dynamics, thermal state preparation, Green's function computation, excited states — are explicitly not addressed and may have different complexity profiles.
- **Polynomial speedups are not ruled out.** Even a modest polynomial advantage (e.g., quadratic) could matter in practice, especially when classical algorithms have large polynomial exponents.
- The classical heuristic evidence is numerical, not asymptotic proof. It's possible (though the authors argue unlikely) that classical methods hit exponential walls at larger system sizes not yet tested.
- The argument assumes the current landscape of classical heuristics is representative. A future discovery of exponential classical hardness in a specific chemical problem would change the picture.
- The paper's Fe-S numerical experiments use active space models. Full ab initio calculations at the same system sizes would be harder for both classical and quantum methods, but the relative comparison likely still holds.
- The paper acknowledges that one can construct artificial Hamiltonians where EQA holds (via circuit-to-Hamiltonian constructions), but argues these don't represent generic chemistry.

---

## Reusable ideas

1. [[ASP Time–Overlap Correlation]] — Adiabatic state preparation time scales as $\mathrm{poly}(1/|\langle \Upsilon_0 | \Psi_0 \rangle|)$ for chemical Hamiltonians, tying ASP cost directly to initial-state quality.
2. [[EQA Assessment Framework (State Preparation vs Classical Heuristic)]] — To establish exponential quantum advantage, you must show *both* that quantum state preparation is exponentially cheaper than classical solution *and* that classical heuristics are exponentially hard. Showing one without the other is insufficient.

---

## References within this paper

Key papers cited:

- Reiher, Wiebe, Svore, Wecker, Troyer (2017) — the original FeMoCo resource estimate that launched "quantum chemistry as killer app"; this paper directly challenges its premise. [No vault note]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — qubitization, QROM, surface-code compilation for chemistry
- Gharibian & Le Gall (2021, arXiv:2111.09079) — BQP-hardness of ground-state energy to inverse-polynomial precision, establishing formal quantum-classical separation under the assumption of good initial state. [No vault note]
- Lin & Tong (2020, Quantum 4, 372) — near-optimal ground state preparation; used for classical binary-search complexity analysis in the SI. [No vault note]
- Lin & Tong (2021, arXiv:2102.11340) — Heisenberg-limited ground state energy estimation for early fault-tolerant quantum computers. [No vault note]
- Kempe, Kitaev, Regev (2006) — QMA-completeness of local Hamiltonian problem; ground-state determination can be exponentially hard *even quantumly*. [No vault note]
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — early hardware QPE for chemistry
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]] — QPE windowing techniques (Kaiser window)
- White & Martin (1999) — ab initio DMRG, the classical method that arguably does best on the Fe-S problems. [No vault note]
- Riplinger, Pinski, Becker, Valeev, Neese (2016) — DLPNO-CCSD(T), the reduced-scaling coupled cluster used for the alkane benchmarks. [No vault note]

---

## Cross-links

- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — original FeMoCo resource estimate; the paper that established FeMoCo as the standard quantum chemistry benchmark

### Related paper notes
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — complementary study of ground-state overlaps; this paper extends the analysis to Fe-S clusters where the catastrophe is severe
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — the state-of-the-art FeMoCo resource estimate that this paper questions the premise of
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — earlier FeMoCo resource estimates
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — qubitization and surface-code compilation, used as the quantum algorithm baseline
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — first-quantized methods with excellent asymptotics, but the EQA question still applies
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — QC-QMC is a hybrid approach that sidesteps full QPE; this paper's findings about classical heuristic strength are relevant to whether QC-QMC's polynomial advantage matters
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — double factorization applied to Fe-S clusters (up to 146 spin-orbitals), providing context for the classical DMRG benchmarks here
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — the [[Extensive vs Intensive Error Targeting]] distinction is directly relevant here, since the paper's argument about $\bar{\varepsilon}$ vs $\varepsilon$ parallels the intensive/extensive error framework
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — similarly concludes that polynomial speedups may be insufficient for practical advantage; parallel pessimism
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — Concrete case study of the quantum advantage question for a pharmaceutically relevant system; supports the "polynomial advantage possible, exponential advantage unlikely" conclusion; demonstrates the classical-quantum runtime crossover for CYP models
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]] — Uses the same Fe-S cluster models (including FeMo-co topology) reduced to $S = 1/2$ Heisenberg models for near-term hardware simulation on Sycamore; demonstrates that even the simplified spin models are at the limit of what current hardware can handle
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — Proposes an alternative avenue for quantum advantage: dynamics rather than ground states. Since this paper finds no evidence for EQA in ground-state chemistry, Babbush et al. (2023) argue that dynamics against mean-field methods is a more promising target for polynomial quantum speedup
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — parallel "does quantum advantage hold?" analysis for TDA; uses the same honest assessment framework
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]]
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — a positive result: exponential quantum advantage does exist for coupled oscillator simulation; contrasts with the negative result here for ground-state chemistry
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes]] — same "where is the quantum advantage?" question applied to QML; training data can elevate classical models, similar to how good ansätze can eliminate the quantum advantage for state preparation
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]] — Directly addresses the state preparation concern raised here: MPS initial states with $\chi = 4000$ give $\sim 0.9$ squared overlap for FeMoCo, keeping total QPE cost within $2.3\times$ of the perfect-overlap estimate. The [[MPS Overlap Extrapolation Protocol]] provides a (non-rigorous) way to estimate overlap without knowing the ground state.

### Related trick cards
- [[ASCI Overlap Estimation for QPE Initial State]] — ASCI-based overlap certification; this paper's Fe-S data shows the limits of simple ansatz states
- [[Extensive vs Intensive Error Targeting]] — the $\bar{\varepsilon}$ vs $\varepsilon$ distinction is central to this paper's cost analysis of classical heuristics
- [[Stroboscopic Adiabatic Walk]] — related to the ASP analysis
- [[Sequential Multi-Determinant State Preparation]] — relevant as a strategy for improving overlap in QPE
- [[ASP Time–Overlap Correlation]] — new trick card extracted from this paper
- [[EQA Assessment Framework (State Preparation vs Classical Heuristic)]] — new trick card extracted from this paper

See [[FeMoCo Resource Estimation Timeline]] for how this estimate fits in the broader progression.
