> **Source:** Ruslan N. Tazhigulov, Shi-Ning Sun, Reza Haghshenas, Huanchen Zhai, Adrian T. K. Tan, Nicholas C. Rubin, Ryan Babbush, Austin J. Minnich, Garnet Kin-Lic Chan, *Simulating challenging correlated molecules and materials on the Sycamore quantum processor*, arXiv:2203.15291, PRX Quantum **3**, 040318 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2203.15291) · [Journal](https://doi.org/10.1103/PRXQuantum.3.040318)
> **Tags:** #quantum-simulation #NISQ #superconducting #error-mitigation #Heisenberg-model #Kitaev-model #spin-liquid #nitrogenase #iron-sulfur #Sycamore #QITE #recompilation #finite-temperature

---

## The computational problem

Compute finite-temperature static and dynamical electronic structure properties — specifically spin-spin correlation functions $\langle Z_i Z_j \rangle_\beta$ and dynamical structure factors $S(\omega) = \mathrm{FT}[\langle Z_i(t) Z_j(0)\rangle_\beta]$ — for low-energy spin models derived from two physically interesting correlated electron systems:

1. **Iron-sulfur clusters of nitrogenase** ([4Fe-4S] and [8Fe-7S] P-cluster/FeMo-cofactor), modelled as $S = 1/2$ Heisenberg models with the Fe-S coupling topology.
2. **$\alpha$-RuCl$_3$**, a proximate spin-liquid material, modelled as a Kitaev-Heisenberg model on a honeycomb lattice (6-site and 10-site).

The key question is practical, not algorithmic: how much of the gate budget available for random circuit sampling "supremacy" experiments can actually be deployed for physically meaningful simulation?

## What the paper does

Tests whether Google's Weber/Sycamore processor can produce quantitatively useful results on spin models derived from real materials, rather than on problems chosen to favor the hardware. The answer is mixed. For circuits up to ~100 two-qubit gates (4-site Fe-S, 6-site $\alpha$-RuCl$_3$), the qualitative physics survives — correct spin pairing patterns, identifiable spectral peaks — but only after extensive error mitigation and classical-data-assisted rescaling. At 310 two-qubit gates (10-site $\alpha$-RuCl$_3$), the simulation fails entirely unless the Hamiltonian parameters are tuned to suit the hardware. The deployable gate budget for physically meaningful results is roughly 1/5 of what random circuit supremacy uses, rising to 1/2 if you pick your model carefully.

My assessment: this is an honest benchmark paper. The Caltech–Google team isn't overselling the hardware. The main takeaway is sobering — the gap between "quantum supremacy on synthetic tasks" and "useful quantum simulation" is wide, and most of it comes from the fact that physical problems demand quantitative accuracy, not just distinguishing a signal from zero. The dependence on classical recompilation and rescaling is the elephant in the room: every step that makes the results better also makes you more dependent on having solved the problem classically first.

## The algorithm / construction

### Spin model derivation

Both systems have electronic structures dominated by low-energy spin excitations. The full electronic models (100+ spin-orbitals for FeMo-co) are far too large for current hardware, so the authors reduce to $S = 1/2$ spin models:

**Fe-S clusters:**

$$\hat{H} = \sum_{\langle ij \rangle} J_{ij} \mathbf{S}_i \cdot \mathbf{S}_j$$

For [4Fe-4S]: two coupling constants $J = 1$, $J' = 1.17J$ (the all-ferric cubane). For the P-cluster and FeMo-co: a single $J$ for all bonds. This simplification means P-cluster and FeMo-co have the same Hamiltonian, which is a significant reduction. The $S = 1/2$ spectrum retains qualitative features of the more physical $S = 5/2$ model (same ground-state total spin and degeneracy), though the energies differ quantitatively.

**$\alpha$-RuCl$_3$:**

$$\hat{H}_{KH} = J \sum_{\langle i,j \rangle} \mathbf{S}_i \cdot \mathbf{S}_j + K_\gamma \sum_{\langle i,j \rangle_\gamma} S_i^\gamma S_j^\gamma + \Gamma \sum_{\langle i,j \rangle}(S_i^\alpha S_j^\beta + S_i^\beta S_j^\alpha) + \Gamma' \sum_{\langle i,j \rangle} \sum_{\alpha \neq \gamma}(S_i^\gamma S_j^\alpha + S_i^\alpha S_j^\gamma)$$

With parameters $J = -1.53$, $K = -24.4$, $\Gamma = 5.25$, $\Gamma' = -0.95$ from the literature. The exactly solvable Kitaev point corresponds to keeping only the $K_\gamma$ term.

### Quantum imaginary time evolution (QITE)

The algorithm prepares thermal states $\rho = e^{-\beta H}/\mathrm{Tr}(e^{-\beta H})$ via QITE. For each computational basis state $|\Psi_i(0)\rangle$:

$$e^{-\beta H/2}|\Psi_i(0)\rangle = c_i(\beta) \cdot U_i(\beta/2)|\Psi_i(0)\rangle$$

where $c_i(\beta)$ is a classically tracked normalization and $U_i(\beta/2)$ is determined by the QITE procedure. Finite-temperature observables are computed as:

$$\langle \hat{A} \rangle_\beta = \frac{\sum_i P_i A_i}{\sum_i P_i}$$

with $P_i = |c_i(\beta)|^2$ and $A_i = \langle \Psi_i(\beta/2)|\hat{A}|\Psi_i(\beta/2)\rangle$. For dynamical correlation functions, the imaginary-time-evolved states are further propagated in real time, and the Hadamard test extracts $\langle A(t)B\rangle_\beta$ using an ancilla qubit.

### Circuit recompilation

The native QITE circuits are far too deep for the hardware. A first-order Trotter expansion with $\Delta t = 0.1$ for the 6-site Kitaev-Heisenberg model already exceeds the random circuit supremacy gate count after just 3 time units. So for each target imaginary/real time point, the exact unitary is constructed classically, then a brickwork circuit of native gates ($\sqrt{iSWAP}^\dagger$ + PhasedXZ) is variationally optimized to minimize $\|U_i|\Psi_i(0)\rangle - \bar{U}_i|\Psi_i(0)\rangle\|$, achieving $>90\%$ fidelity in noiseless classical emulation.

This is the critical weakness: **the classical recompilation requires solving the problem classically first.** The quantum device is then asked to reproduce a classically known answer through a compressed circuit. As the authors acknowledge, this raises questions about extending the approach to classically intractable systems.

### Error mitigation stack

The paper layers five error mitigation techniques:

| Technique | What it does |
|---|---|
| **Floquet calibration** | Calibrates actual $\sqrt{iSWAP}^\dagger$ gate parameters (5 angles: $\theta, \phi, \zeta, \gamma, \chi$) before each experiment; feeds corrected parameters into recompilation. Only calibrates isolated gates — coherent errors from adjacent microwave gates are not captured. |
| **Readout error mitigation** | Standard readout confusion matrix correction. |
| **Postselection** | The Fe-S and simplified $\alpha$-RuCl$_3$ Hamiltonians have $Z_2$ symmetry; shots violating the symmetry are discarded. Related to [[Number-Basis Postselection for Error Mitigation]] and [[Symmetry-Based QSE for Unencoded Error Mitigation]]. |
| **Dynamical decoupling** | XX and YY identity insertions on the idle ancilla qubit during the Hadamard test, mitigating decoherence during long idle periods. |
| **Classical rescaling** | The most aggressive technique. A commuting sub-Hamiltonian $\hat{H}'$ (e.g., only terms on pairs (1-2), (3-4), (5-6)) is simulated both exactly classically and on the hardware using the same circuit ansatz. The ratio $f(t) = \langle A\rangle_t^{\mathrm{ideal}}(\hat{H}') / \langle A\rangle_t^{\mathrm{hw}}(\hat{H}')$ rescales the full Hamiltonian results. This works because the circuit ansatz couples all qubits even for the commuting Hamiltonian, so the noise profile is similar. |

The rescaling technique is a form of [[Commuting Hamiltonian Rescaling for Error Mitigation|commuting-Hamiltonian rescaling]]: clever, but it requires classical knowledge of the answer for a related problem and assumes the noise structure transfers between problems.

## Key results

### Resource deployment

| System | Qubits (incl. ancilla) | Two-qubit gates | Single-qubit gates | Outcome |
|---|---|---|---|---|
| [4Fe-4S] static | 5 | — | — | Correlation functions within ~4% of exact |
| [4Fe-4S] dynamic | 5 | 22 | — | Frequencies correct; amplitudes ~50% error before rescaling, ~0% after |
| P-cluster/FeMo-co dynamic | 9 | 82 | — | Frequencies mostly correct; largest peak 60% suppressed; ~20% error after rescaling |
| $\alpha$-RuCl$_3$ 6-site energy + $c_v$ | 7 | ~64 | — | Two-peak heat capacity visible but extremely noisy |
| $\alpha$-RuCl$_3$ 6-site dynamic | 7 | 64 | — | Qualitatively correct after full mitigation |
| $\alpha$-RuCl$_3$ 10-site dynamic | 11 | 310 | 782 | **Failed.** Data too degraded even after all mitigation. |
| $\alpha$-RuCl$_3$ 10-site (anisotropic $K$) | 11 | 310 | 782 | Recoverable after rescaling — demonstrating model-hardware matching. |

### Random circuit supremacy comparison

Random circuit supremacy on Sycamore used ~50 qubits and ~500 two-qubit gates. This paper's successful simulations used at most ~100 two-qubit gates on ~9 qubits — roughly **1/5 of the supremacy gate budget**. The anisotropic Kitaev model (hardware-friendly parameters) pushed this to **~1/2**. The discrepancy reflects that physical simulation requires quantitative accuracy (correlations, spectral weights), while supremacy only requires distinguishing a signal from zero.

### Noise characterization

The unmitigated [4Fe-4S] results are well-reproduced by a depolarizing noise model with per-moment error rate $p \approx 0.005$–$0.010$ (Fig. S3). This is consistent with individual gate error rates of $\sim 0.5\%$.

## Comparison with prior work

| Paper | Year | System | Qubits | Gates | Algorithm | Classical assistance? |
|---|---|---|---|---|---|---|
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes\|O'Malley et al.]] | 2016 | H₂ | 2–3 | 2–14 | [[Variational Quantum Eigensolver (VQE)\|VQE]]/PEA | Minimal (classical optimizer) |
| Kandala et al. | 2017 | H₂, LiH, BeH₂ | 6 | ~50 | VQE | Classical optimizer |
| [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes\|Huggins et al.]] | 2022 | Diamond, carbon chain | 8–16 | ~50–200 | QC-AFQMC | Shadow tomography + classical QMC |
| Arute et al. | 2020 | Free fermion dynamics | ~12 | ~500 | Direct Trotter | No microwave gates needed |
| **This paper** | 2022 | Fe-S clusters, $\alpha$-RuCl$_3$ | 5–11 | 22–310 | QITE + recompilation | Full classical recompilation + rescaling |

The Arute et al. (2020) free-fermion experiment deployed more gates because it avoided microwave (single-qubit) gates entirely, sidestepping the Floquet calibration issue. The Huggins QC-QMC work achieved practical results on larger basis sets (up to 120 orbitals) using a fundamentally different paradigm — the quantum device provides a trial wavefunction rather than directly simulating dynamics.

## Limits / caveats

1. **Classical recompilation kills scalability.** The entire procedure requires constructing the exact unitary classically before compressing it into a hardware circuit. For classically intractable systems, you can't do this. The authors acknowledge this explicitly.

2. **Classical rescaling requires a solvable reference.** The commuting sub-Hamiltonian approach works because the noise profile transfers, but finding an appropriate $\hat{H}'$ that is both classically solvable and noise-similar becomes harder for larger systems.

3. **The $S = 1/2$ reduction is severe.** Nitrogenase Fe centers have $S = 2$ or $S = 5/2$; reducing to $S = 1/2$ preserves qualitative features but changes the spectrum quantitatively. The reduction was necessary to fit the problem on the device, but it means the "real-world" claim is somewhat stretched.

4. **10-site $\alpha$-RuCl$_3$ failed.** At 310 two-qubit gates, the data is unrecoverable. This sets a clear upper bound on what the hardware can do for this class of problems.

5. **Floquet calibration is incomplete.** It calibrates isolated two-qubit gates, but cross-talk from adjacent microwave (single-qubit) gates introduces uncharacterized errors. This is a hardware-level limitation that affects all brickwork circuit architectures.

6. **All "successful" simulations are classically easy.** The 4-site Heisenberg model has $2^4 = 16$ basis states; the 10-site Kitaev-Heisenberg model has $2^{10} = 1024$. None of these are remotely classically hard. The value of the paper is as a benchmark, not as a computational advance.

## Reusable ideas

1. [[Commuting Hamiltonian Rescaling for Error Mitigation]] — Using a classically solvable commuting sub-Hamiltonian run through the same circuit ansatz to derive time-dependent rescaling factors for the full-Hamiltonian results.

2. [[Floquet Gate Calibration for Circuit Recompilation]] — Characterizing the actual parameters of native two-qubit gates via Floquet calibration, then feeding corrected gate definitions into the classical circuit optimization, rather than assuming ideal gate implementations.

3. [[Classical Circuit Recompilation for Depth Reduction]] — Variationally compressing an exact unitary (known classically) into a fixed-depth brickwork circuit of native gates, optimizing state-specific fidelity $\|U|\psi\rangle - \bar{U}|\psi\rangle\|$. Not scalable, but useful as a benchmark tool.

## References within this paper

- **Motta et al. (2020), Nature Physics 16, 205** — Introduces QITE (quantum imaginary time evolution), the formal algorithm used here. [No vault note]
- **Sun et al. (2021), PRX Quantum 2, 010317** — Introduces QITE with classical recompilation, the practical variant used here. [No vault note]
- Arute et al. (2019), Nature 574, 505 — Random circuit sampling supremacy on Sycamore. [No vault note]
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — Earlier superconducting quantum chemistry experiment.
- **Arute et al. (2020), arXiv:2010.07965** — Free-fermion dynamics on Sycamore; higher gate counts possible because no microwave gates needed.
- **Neill et al. (2021), Nature 594, 508** — Floquet calibration methodology used here.
- **Bauer, Bravyi, Motta, Chan (2020), Chemical Reviews 120, 12685** — Review of quantum simulation for chemistry.
- **Sharma, Sivalingam, Neese, Chan (2014), Nature Chemistry 6, 927** — Ab initio electronic structure of Fe-S clusters revealing dense low-lying spin manifold.

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — Original VQE paper that opened hardware quantum chemistry; this paper asks the same question (what can near-term hardware do for chemistry?) a decade later with far more qubits and error mitigation
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]] — Variational error suppression theory; the QITE+recompilation approach here is a different near-term strategy reaching similar conclusions about error tolerability
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — Uses the same Fe-S cluster models to study ground-state overlap decay. The overlap catastrophe found there (HF overlap $\sim 10^{-7}$ for FeMo-co) is what makes fault-tolerant simulation of these systems hard. This paper takes the opposite approach: forget about the full electronic structure, reduce to a spin model, and see what near-term hardware can do.
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes]] — Another Sycamore experiment on correlated chemistry (QC-AFQMC); fundamentally different paradigm where the quantum device supplies a trial wavefunction rather than directly simulating dynamics.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — The original superconducting quantum chemistry experiment. This paper is the 2022 version of the same question: what can the hardware actually do?
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]] — Double factorization applied to Fe-S clusters in the full electronic structure model (up to 146 spin-orbitals). Contrasts with the spin-model reduction used here.
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Develops number-basis postselection and readout error mitigation techniques. The symmetry postselection used in this paper is a simpler version of the same idea.
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]] — Symmetry-based error mitigation framework that the postselection technique in this paper draws from.

### Trick cards
- [[Commuting Hamiltonian Rescaling for Error Mitigation]] — new card
- [[Floquet Gate Calibration for Circuit Recompilation]] — new card
- [[Classical Circuit Recompilation for Depth Reduction]] — new card
- [[Number-Basis Postselection for Error Mitigation]] — related symmetry postselection approach
- [[Symmetry-Based QSE for Unencoded Error Mitigation]] — broader error mitigation via symmetries
- [[Variational Quantum Eigensolver (VQE)]] — related NISQ paradigm
- [[Trotterized Time Evolution for Chemistry]] — the Trotter steps that recompilation replaces
