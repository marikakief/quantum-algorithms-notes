> **Source:** Markus Reiher, Nathan Wiebe, Krysta M. Svore, Dave Wecker, Matthias Troyer, *Elucidating Reaction Mechanisms on Quantum Computers*, arXiv:1605.03590, PNAS **114**(29):7555–7560 (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1605.03590) · [Journal](https://doi.org/10.1073/pnas.1619152114)
> **Tags:** #quantum-chemistry #resource-estimation #fault-tolerant #FeMoCo #nitrogenase #Trotter-Suzuki #phase-estimation #surface-code #T-gate #quantum-simulation

---

## The computational problem

Given the second-quantized electronic Hamiltonian for a molecular active space:

$$H = \sum_{pq} h_{pq}\, a^\dagger_p a_q + \frac{1}{2}\sum_{pqrs} h_{pqrs}\, a^\dagger_p a^\dagger_q a_r a_s$$

compute the ground-state energy to chemical accuracy (0.1 mHa target, 1 mHa for qualitative accuracy) using [[Quantum Phase Estimation|quantum phase estimation]] (QPE) with [[Product Formula (Trotter-Suzuki)|Trotter-Suzuki]] time evolution, compiled into the Clifford+T gate set under the [[Surface Code|surface code]]. The target system is the iron-molybdenum cofactor (FeMoCo) of nitrogenase — a Fe$_7$MoS$_9$C cluster with 54–65 active electrons in 54–57 spatial orbitals (108–114 spin-orbitals).

---

## What the paper does

This is the paper that turned "quantum computers might be useful for chemistry" from a vague aspiration into a concrete engineering target. Reiher et al. take a real, unsolved chemistry problem — the mechanism of biological nitrogen fixation at the FeMoCo active site of nitrogenase — and work out, in detail, what it would take to solve it on a fault-tolerant quantum computer. The answer: ~$10^{14}$ T gates, ~111 logical qubits, and ~130 days of runtime for quantitative accuracy using second-order Trotter-Suzuki, or ~$10^{15}$ T gates and ~15 days with nesting parallelism. With ~$10^5$–$10^6$ physical qubits at error rates of $10^{-6}$–$10^{-9}$.

The paper matters not because these estimates were tight (they weren't — subsequent work brought the gate count down by five orders of magnitude). It matters because it was the first to combine a scientifically important target molecule, a complete computational workflow (from DFT structure optimization through QPE energy evaluation), and an honest fault-tolerant resource accounting. It gave the field a benchmark everyone could aim at.

---

## The hybrid classical-quantum workflow

The paper lays out a practical workflow for using quantum computers in chemistry that remains the template for most subsequent resource estimation papers:

1. **Structure generation and optimization** — done classically with DFT (B3LYP/def2-TZVP). Quantum computers don't help here.
2. **Orbital optimization** — small-CAS CASSCF to get natural orbitals. Classically tractable.
3. **Four-index integral transformation** — classical preprocessing to get $h_{pq}$ and $h_{pqrs}$ in the molecular orbital basis.
4. **Ground-state energy via QPE on a quantum computer** — the expensive step. CAS-FCI in the selected active space.
5. **Temperature/entropy corrections** — classical post-processing via standard thermochemistry protocols.
6. **Kinetic modeling** — energy differences feed into Eyring absolute rate theory for reaction rates.

The key insight: the quantum computer is an accelerator for step 4 only. Everything else runs classically. This "quantum-as-coprocessor" model is now standard but was clearly articulated here.

---

## The FeMoCo system

The iron-molybdenum cofactor is the active site of Mo-dependent nitrogenase, which fixes atmospheric N$_2$ to NH$_3$ under ambient conditions — a reaction the industrial Haber-Bosch process requires extreme temperatures and pressures to achieve (consuming ~2% of global energy production). The mechanism remains unknown because:

- FeMoCo has 7 iron atoms and 1 molybdenum atom bridged by sulfur, with a central carbon — a nightmare for electronic structure methods
- Multiple charge states, spin states, and protonation states must be explored
- Strong static correlation from dense d-electron manifold makes DFT unreliable for energies
- CASSCF is limited to ~18 orbitals classically; DMRG can push to ~100 but convergence is neither easy nor guaranteed

Two structures are studied:

| | Structure 1 | Structure 2 |
|---|---|---|
| Spin state $S$ | 0 | 1/2 |
| Charge | +3 | 0 |
| Active electrons | 54 | 65 |
| Spatial orbitals | 54 | 57 |
| Spin-orbitals | 108 | 114 |

Active spaces were selected via Pulay's UNO-CAS criterion from small-CAS CASSCF natural orbital occupation numbers.

---

## The algorithm

### Trotter-Suzuki decomposition

The Hamiltonian is mapped to qubits via the [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner transformation]] and time evolution is approximated by the second-order (Strang splitting) [[Product Formula (Trotter-Suzuki)|Trotter-Suzuki formula]]:

$$e^{-iHt} = \left(\prod_{j=1}^{M} e^{-iH_j t/2r} \prod_{j=M}^{1} e^{-iH_j t/2r}\right)^r + O(t^3/r^2)$$

where $M \sim 6.1 \times 10^6$ terms for the 54-orbital basis.

### Phase estimation

Uses iterative phase estimation with a controlled-evolution trick (Lemma 2 in the paper) that effectively doubles the phase sensitivity. Instead of implementing $\Lambda(U^M)$ (controlled-$U$ repeated $M$ times), they implement $W^M$ where $W|0\rangle|\psi\rangle = |0\rangle U|\psi\rangle$ and $W|1\rangle|\psi\rangle = |1\rangle U^\dagger|\psi\rangle$. This gives a measurement probability:

$$P(0|\phi;\theta,M) = \frac{1 + \cos(2M[\phi + \theta])}{2}$$

— doubling the phase accumulation per application, halving the number of unitaries needed. The expected cost is $M(\varepsilon) \approx 2.3/\varepsilon$ using RFPE (Bayesian phase estimation).

### Circuit synthesis

Single-qubit rotations compiled into Clifford+T using:
- **Serial case:** ancilla-free synthesis, $C(\varepsilon) \approx 4\log_2(1/\varepsilon) + 11$ T gates per rotation
- **PAR (Programmable Ancilla Rotations):** teleport pre-cached rotations into the circuit, reducing T-depth at the cost of more qubits and T factories. Uses a geometric retry distribution with $n$ cached correction angles per rotation.

### Error budget optimization

Three sources of error are jointly optimized:
- $\varepsilon_1$: phase estimation statistical error
- $\varepsilon_2 = \Delta E_{\text{TS}}$: Trotter-Suzuki systematic error
- $\varepsilon_3 = (2M-1)\Delta_{\text{synth}}/t$: circuit synthesis error

Subject to $\varepsilon_1 + \varepsilon_2 + \varepsilon_3 \leq 10^{-4}$ Ha. The cost function (Eq. E2 in the paper) is minimized over the error allocation — an early example of what's now standard [[Error Budget Optimization for Fault-Tolerant Algorithms|error budget optimization]].

### Three parallelism strategies

| Approach | Description | T-depth reduction | Qubit overhead |
|---|---|---|---|
| **Serial** | All rotations sequential | Baseline | 111 logical qubits |
| **Nesting** | Commuting terms executed in parallel; ~26–28 terms grouped | ~26× | ~135 logical qubits |
| **PAR** | Programmable ancilla rotations; pre-cached in T factories | ~1000× | ~1982 logical qubits |

---

## Key results

### Logical-level estimates (0.1 mHa accuracy)

| | Serial | Nesting | PAR |
|---|---|---|---|
| **T gates (Struct. 1)** | $1.1 \times 10^{15}$ | $3.5 \times 10^{15}$ | $3.1 \times 10^{16}$ |
| **Clifford gates (Struct. 1)** | $1.7 \times 10^{15}$ | $5.7 \times 10^{15}$ | $3.1 \times 10^{16}$ |
| **Time (Struct. 1)** | 130 days | 15 days | 110 hours |
| **Logical qubits (Struct. 1)** | 111 | 135 | 1,982 |
| **T gates (Struct. 2)** | $2.0 \times 10^{15}$ | $6.5 \times 10^{15}$ | $6.0 \times 10^{16}$ |
| **Time (Struct. 2)** | 240 days | 27 days | 204 hours |
| **Logical qubits (Struct. 2)** | 117 | 142 | 2,024 |

At 1 mHa (qualitative accuracy), all costs drop by ~10×.

### Surface code estimates (0.1 mHa, Structure 1)

| | Serial | Nesting | PAR |
|---|---|---|---|
| **Phys. qubits ($p = 10^{-3}$)** | $1.8 \times 10^8$ | $5.1 \times 10^9$ | $1.8 \times 10^{11}$ |
| **Phys. qubits ($p = 10^{-6}$)** | $1.2 \times 10^6$ | $3.0 \times 10^7$ | $3.1 \times 10^9$ |
| **Phys. qubits ($p = 10^{-9}$)** | $2.3 \times 10^5$ | $5.2 \times 10^6$ | $1.5 \times 10^8$ |
| **T factories** | 202 / 68 / 38 | 5,845 / 1,842 / 1,029 | 166,462 / 41,110 / 29,659 |

The paper also considers topological qubits (Ising anyons) with protected Clifford gates but noisy T states ($p_T = 10^{-4}$), finding modest but real savings.

### Trotter number estimates

The paper uses Monte Carlo sampling of the double-commutator upper bound (Eq. C5) to estimate Trotter numbers, then considers four scaling scenarios:

| Scenario | Trotter number $\beta$ (54 orbitals) | T gates (Serial) | Time |
|---|---|---|---|
| Rigorous upper bound | $7 \times 10^6$ | $1.0 \times 10^{21}$ | $3.2 \times 10^5$ years |
| Pessimistic rescaling | 1,075 | $7.9 \times 10^{15}$ | 30 months |
| Average rescaling | 166 | $1.2 \times 10^{15}$ | 135 days |
| Optimistic (data fit) | 24 | $1.6 \times 10^{14}$ | 19 days |

The "rescaled" estimates (dividing the upper bound by the average discrepancy observed for small molecules) are what Table I in the main paper reports.

---

## How loose these estimates turned out to be

This is the elephant in the room, and the reason this paper spawned an entire subfield of quantum chemistry resource estimation. The original Reiher et al. estimates were *extremely* conservative — not because of carelessness, but because of the tools available in 2016. Every subsequent paper in the vault that tackles FeMoCo is, in some sense, an answer to the question "how much better can we do?"

### The FeMoCo Toffoli count timeline

| Year | Paper | Method | Toffoli/T count (Reiher Hamiltonian) | Improvement vs. Reiher |
|---|---|---|---|---|
| 2017 | **This paper** | 2nd-order Trotter + QPE | $\sim 10^{14}$ (rescaled serial) | Baseline |
| 2018 | [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al.]] | [[Qubitization (Quantum Walk for Spectral Encoding)\|Qubitization]] + [[QROM (Quantum Read-Only Memory)\|QROM]] | — (plane-wave basis; ~$10^6$ qubits estimated for FeMoCo-scale) | Architecture shift |
| 2019 | [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry, Gidney, Motta et al.]] | Sparse Coulomb qubitization | $8.8 \times 10^{10}$ | ~$1{,}000\times$ |
| 2020 | von Burg et al. | Double factorization qubitization | $1.0 \times 10^{10}$ | ~$10{,}000\times$ |
| 2021 | [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes\|Lee, Berry, Babbush et al.]] | [[THC Non-Orthogonal Diagonal Coulomb Representation\|THC]] qubitization | $5.3 \times 10^{9}$ | ~$20{,}000\times$ |
| 2024 | Oumarou et al. | Symmetry-compressed DF | $2.6 \times 10^9$ | ~$40{,}000\times$ |
| 2025 | [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes\|Low, King, Berry et al.]] | DFTHC + BLISS + spectrum amplification | $3.4 \times 10^8$ | ~$300{,}000\times$ |

Five orders of magnitude in eight years.

### Why the original estimates were so loose

The looseness was structural, not a matter of sloppy accounting. Several independent sources of overhead stacked multiplicatively:

1. **Trotter-based simulation.** The 2nd-order Trotter-Suzuki formula has $O(N^{10} t^2)$ worst-case error scaling for $N^4$ Hamiltonian terms. The actual Trotter numbers for FeMoCo were extrapolated from small molecules with a ~10,000× gap between upper bounds and exact values — the "rescaled" estimates were educated guesses. [[Qubitization (Quantum Walk for Spectral Encoding)|Qubitization]] (introduced for chemistry in [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush-Gidney 2018]]) eliminated Trotter error entirely, achieving Heisenberg-limited precision ($O(1/\varepsilon)$ instead of $O(1/\varepsilon)$ with a much better constant).

2. **No tensor factorization of integrals.** Reiher et al. treat the $O(N^4)$ two-electron integrals as given. Subsequent work discovered that factoring the Coulomb tensor — via [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|double factorization]], single factorization, and [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|tensor hypercontraction]] — dramatically reduces both the 1-norm $\lambda$ (which controls qubitization cost) and the number of parameters the quantum computer needs to load.

3. **Rotation synthesis overhead.** Each Trotter step requires $O(N^4)$ arbitrary-angle single-qubit rotations, each costing $O(\log(1/\varepsilon))$ T gates. Qubitization replaces these with Toffoli-based QROM lookups, eliminating rotation synthesis entirely for the inner loop.

4. **State preparation was ignored.** The paper explicitly defers the cost of preparing an initial state with good ground-state overlap — noting Hartree-Fock overlaps of ~43% for FeMoCo-scale molecules. [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes|Berry, Tong et al. (2024)]] finally addressed this with MPS-based state preparation, finding that imperfect overlap costs only ~$2.3\times$ in total Toffolis.

5. **No optimized QPE.** The paper uses standard iterative QPE with RFPE. More recent work uses [[Kaiser Window Amplitude Estimation|Kaiser/Slepian window-optimized QPE]] to reduce the cost of distinguishing the ground state from excited states.

**My assessment:** The paper was honestly conservative about every uncertain quantity, and made the right call — better to overestimate and be pleasantly surprised than to undercount and mislead the field. The result is that FeMoCo became the standard benchmark *precisely because* the original estimates left so much room for improvement. Every new algorithmic idea — qubitization, THC, double factorization, spectrum amplification — could demonstrate its value by pushing the FeMoCo numbers down. The paper's lasting contribution isn't its resource estimates; it's the benchmark itself.

---

## Comparison with prior work

| Aspect | Pre-Reiher state of the art | This paper |
|---|---|---|
| Target molecule | Small molecules (H₂O, LiH, ~20 spin-orbitals) | FeMoCo (108–114 spin-orbitals) |
| Chemistry workflow | QPE on toy systems | Full DFT→CASSCF→QPE→kinetics pipeline |
| Error accounting | Often omitted rotation synthesis costs | Includes Trotter, QPE, and synthesis errors jointly optimized |
| Physical resource count | Logical-level only | Full surface code compilation with T factories |
| Basis set quality | Minimal | def2-TZVP (production quality for this chemistry) |

Prior papers on quantum chemistry simulation — Aspuru-Guzik et al. (2005), [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. (2014)]], Wecker et al. (2014), [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush et al. (2015)]] — had either focused on asymptotic scaling, small demonstration systems, or partial resource counts. This was the first to do the full stack for a real molecule.

---

## Limits / caveats

- **State preparation cost is not included.** The paper acknowledges this explicitly but doesn't resolve it. For a strongly correlated system like FeMoCo, Hartree-Fock overlap could be very low.
- **Trotter error estimates are extrapolations.** The upper bounds are computed for small molecules and rescaled by observed discrepancies — an educated guess, not a proof. The paper's own "rigorous" estimates give $10^{21}$ T gates (hundreds of thousands of years), which shows how sensitive the result is to assumptions.
- **Active space selection is assumed.** The 54-orbital active space is chosen by Pulay's UNO-CAS criterion. Different active space choices could change the resource requirements significantly.
- **Dynamic correlation is not fully addressed.** The paper discusses range-separated DFT-CASSCF hybrid approaches but doesn't include their cost in the resource estimate. In practice, capturing dynamic correlation beyond the active space remains an open problem.
- **No comparison with classical methods on the same Hamiltonian.** Later papers ([[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al. 2022]], [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings et al. 2022]]) showed that classical methods (DMRG, CCSD(T)) can be competitive for moderate active spaces, raising the bar for quantum advantage.
- **100 MHz physical gate rate assumption.** This was reasonable in 2016 but modern superconducting architectures operate at ~1 MHz surface code cycle times, changing the runtime calculus.

---

## Reusable ideas

1. [[Hybrid Classical-Quantum Chemistry Workflow]] — the DFT→orbital optimization→QPE→kinetics pipeline. Step 4 runs on the quantum computer; everything else is classical. This decomposition is now standard in every quantum chemistry resource estimation paper.

2. [[Sign-Conditioned Phase Estimation (Ito-Wiebe Trick)]] — replace controlled-$U$ in QPE with $W$ where $W|0\rangle|\psi\rangle = |0\rangle U|\psi\rangle$, $W|1\rangle|\psi\rangle = |1\rangle U^\dagger|\psi\rangle$. Doubles the phase sensitivity, halving the number of unitary applications. Requires only CNOT gates and sign-flipped rotations.

3. [[Programmable Ancilla Rotations (PAR) for T-Depth Reduction]] — pre-cache rotations as $R_z(2^j\theta)|+\rangle$ states in dedicated factories, teleport into the circuit. Geometric retry distribution gives expected T-depth reduction of $2^n$ with $n$ correction levels cached. Trades T-depth for space (qubits and factories).

4. [[Error Budget Optimization for Fault-Tolerant Algorithms]] — jointly optimize the allocation of total error budget across QPE statistical error, Trotter systematic error, and rotation synthesis error. Minimizing the cost function over these three parameters gives substantially better resource estimates than naive equal splitting. This three-way optimization appears in nearly every subsequent resource estimation paper.

5. [[Modular Quantum Computer Architecture for Chemistry]] — separate the quantum computer into a small general-purpose processor, dedicated T factories, and rotation factories. Most qubits go to factories, not the main processor. The factories are special-purpose and can be mass-produced.

---

## References within this paper

### Papers with vault notes
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush, McClean, Wecker, Aspuru-Guzik, Wiebe (2015)]] — Trotter error analysis for quantum chemistry
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve, Sanders (2007)]] — sparse Hamiltonian simulation
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry, Childs, Cleve, Kothari, Somma (2015)]] — truncated Taylor series for Hamiltonian simulation
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo, McClean et al. (2014)]] — VQE (an alternative approach)

### Key uncited but contextually important
- Lloyd (1996) — universal quantum simulators, Ref. [5]
- Zalka (1998) — quantum simulation of quantum systems, Ref. [6]
- Aspuru-Guzik et al. (2005) — first quantum chemistry algorithm proposal, Ref. [9] (this paper appears in the vault under a different title)
- Fowler et al. (2012) — surface code resource estimates, Ref. [48]
- Bravyi and Kitaev (2005) — magic state distillation (15-to-1 protocol), Ref. [50]
- Jones et al. (2012) — PAR rotations, Ref. [12]
- Poulin, Hastings, Wecker, Wiebe, Troyer (2015) — Trotter step size for quantum chemistry, Ref. [17]
- White (1992) — DMRG, Ref. [40]

---

## Cross-links

### Paper notes
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — introduced qubitization for chemistry; first major algorithmic improvement over Trotter for FeMoCo-scale problems
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]] — sparse Coulomb + single factorization qubitization; ~$1000\times$ reduction in Toffolis for FeMoCo
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — double factorization of two-electron integrals
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — THC qubitization; ~$20{,}000\times$ improvement on FeMoCo
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]] — finally addresses the state preparation problem this paper deferred
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]] — current best FeMoCo estimate: $3.4 \times 10^8$ Toffolis
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]] — extends the Reiher-style resource estimation to a different enzyme system
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — resource estimation for non-chemistry (plasma physics) applications using the same workflow
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — critically examines whether quantum advantage is actually exponential for chemistry
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — direct predecessor for the Trotter error analysis
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — improved Trotter error bounds that would tighten the estimates here
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]] — NISQ experiments on Fe-S clusters, the same class of molecules
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — concurrent work on first-quantized resource estimation
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — studies overlap with HF states for FeMoCo and related systems
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — alternative first-quantized approach to the same problem class
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]] — modern compilation framework that can re-analyze algorithms like this one
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]] — unrelated algorithm, but cites FeMoCo as motivating example for quantum utility
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — alternative hybrid quantum-classical approach to chemistry
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]] — alternative basis set approach to the same problem class

### Trick cards
- [[Hybrid Classical-Quantum Chemistry Workflow]]
- [[Sign-Conditioned Phase Estimation (Ito-Wiebe Trick)]]
- [[Programmable Ancilla Rotations (PAR) for T-Depth Reduction]]
- [[Error Budget Optimization for Fault-Tolerant Algorithms]]
- [[Modular Quantum Computer Architecture for Chemistry]]
- [[Adiabatic State Preparation for Chemistry]] — discussed as a state preparation approach in Appendix F
- [[Classical-Quantum Runtime Crossover Analysis]] — the paper implicitly performs this by comparing to DMRG/CASSCF limits
- [[ASCI Overlap Estimation for QPE Initial State]] — related to the state preparation gap this paper leaves open
- [[MPS Overlap Extrapolation Protocol]] — addresses the overlap question deferred here
- [[Sequential Multi-Determinant State Preparation]] — another approach to the state preparation problem
