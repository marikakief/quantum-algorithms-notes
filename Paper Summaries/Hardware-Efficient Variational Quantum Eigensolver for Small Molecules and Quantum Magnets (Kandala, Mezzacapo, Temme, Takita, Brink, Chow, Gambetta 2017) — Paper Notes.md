> **Source:** Abhinav Kandala, Antonio Mezzacapo, Kristan Temme, Maika Takita, Markus Brink, Jerry M. Chow, Jay M. Gambetta, *Hardware-efficient variational quantum eigensolver for small molecules and quantum magnets*, arXiv:1704.05018, Nature **549**, 242–246 (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1704.05018) · [Journal](https://doi.org/10.1038/nature23879)
> **Tags:** #VQE #NISQ #superconducting #quantum-chemistry #hardware-efficient #IBM #H2 #LiH #BeH2 #quantum-magnets

---

## The computational problem

Compute ground-state energies for molecular Hamiltonians (H₂, LiH, BeH₂) and a quantum magnet Hamiltonian on a 6-qubit superconducting quantum processor. Target: demonstrate that variational algorithms with hardware-specific ansätze can outperform physically motivated ansätze (UCC) in terms of circuit depth and gate efficiency on near-term hardware.

Formally: minimize $E(\theta) = \langle \psi(\theta) | H | \psi(\theta) \rangle$ over circuit parameters $\theta$ on hardware subject to native gate constraints (cross-resonance architecture).

---

## What the paper does

Kandala et al. introduce the **hardware-efficient VQE** (HE-VQE) paradigm: instead of constructing circuits motivated by the physics of the ansatz (as in UCC), design the parameterized circuit to match the native gate set and connectivity of the target quantum processor. On IBM's cross-resonance superconducting processors, the native two-qubit gate is the cross-resonance (CR) or CNOT gate operating between adjacent qubits.

The hardware-efficient ansatz is a brick-wall pattern of one-qubit rotations interspersed with CNOT layers:

$$|\psi(\theta)\rangle = \prod_{d=1}^D U_{\rm 2q}^{(d)} \prod_i R_i^{(d)}(\theta) |0\rangle^{\otimes n}$$

where $R_i^{(d)}(\theta) = R_Z(\theta_{i,1}^d) R_Y(\theta_{i,2}^d) R_Z(\theta_{i,3}^d)$ and $U_{\rm 2q}^{(d)}$ implements CNOT on adjacent pairs.

This is the paper that established HE-VQE as the standard baseline for NISQ experiments. It also demonstrates the first VQE experiment on a larger system than H₂ or HeH⁺ — pushing to LiH (4 qubits after reduction) and BeH₂ (6 qubits).

---

## The algorithm / construction

### Fermion-to-qubit mapping and reduction

Hamiltonians are mapped using the parity basis (Bravyi-Kitaev transformation variant). For each molecule:

| Molecule | Molecular orbitals | Qubits before reduction | Qubits used |
|---|---|---|---|
| H₂ | 2 | 4 | 2 (after parity reduction) |
| LiH | 4 | 8 | 4 (freeze core, apply symmetry) |
| BeH₂ | 5 | 10 | 6 (freeze core, apply symmetry) |

The compact encoding uses the fact that particle number parity and electron-spin parity are conserved to eliminate 2 qubits per molecule via symmetry sector projection.

### Hardware-efficient ansatz

The ansatz for $n$ qubits and depth $d$ consists of:
1. $n$ blocks of $R_Z R_Y R_Z$ single-qubit rotations (3 parameters per qubit per layer)
2. A layer of CNOT gates in an entangling pattern (linear chain for this hardware)
3. Repeat $d$ times

For BeH₂ on 6 qubits, depth $d=3$ gives $\sim 54$ rotation angles. The total number of two-qubit gates is $\sim 50$.

### Measurement and optimization

The Hamiltonian is measured as a sum of Pauli strings:
$$H = \sum_k c_k P_k$$

Each Pauli string $P_k$ is measured by applying single-qubit basis rotations and measuring in the $Z$ basis. The energy estimate is $E(\theta) = \sum_k c_k \langle P_k \rangle$.

**Optimization:** Uses a direct classical optimization (SPSA — simultaneous perturbation stochastic approximation) with finite-difference gradient estimates. At each optimization step, $O(\text{params})$ circuit evaluations are needed.

**Error mitigation:** Readout error mitigation using a calibration matrix for bit-flip errors. This is one of the first systematic uses of readout error mitigation in a VQE experiment.

### Quantum magnet application

The same HE-VQE is applied to the transverse-field Ising Hamiltonian:
$$H = -J \sum_{\langle i,j \rangle} Z_i Z_j + h \sum_i X_i$$

on 2D lattice geometries. This demonstrates VQE's generality beyond quantum chemistry.

---

## Key results

**Energy accuracy:**

| Molecule | Qubits | Achieved accuracy (Hartree) | Chemical accuracy? |
|---|---|---|---|
| H₂ | 2 | $\sim 10^{-3}$ | Yes |
| LiH | 4 | $\sim 10^{-2}$ | Qualitative |
| BeH₂ | 6 | $\sim 3 \times 10^{-2}$ | No |

Chemical accuracy ($< 1.6 \times 10^{-3}$ Ha) is achieved for H₂, approximately reached for LiH, and not reached for BeH₂. The BeH₂ failure is attributed to hardware noise at the required circuit depth.

**Gate counts:**
- H₂ (2-qubit circuit): ~10 CNOT gates
- LiH (4-qubit circuit): ~25 CNOT gates
- BeH₂ (6-qubit circuit): ~50 CNOT gates

**Key finding:** The hardware-efficient ansatz can reach lower energies than UCCSD at the same circuit depth on this hardware, because HE-VQE uses the native two-qubit gate set without decomposition overhead.

---

## Comparison with prior work

| Paper | Hardware | Molecule | Qubits | Algorithm | Chemical accuracy? |
|---|---|---|---|---|---|
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. 2014]] | Photonic | HeH⁺ | 2 | VQE+UCC | Yes |
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley et al. 2016]] | Superconducting | H₂ | 2 | VQE+UCC | Yes |
| **Kandala et al. 2017** | **Superconducting** | **H₂, LiH, BeH₂** | **2–6** | **HE-VQE** | **H₂: Yes; LiH, BeH₂: No** |

Prior VQE experiments used physics-motivated ansätze (UCC). This paper introduces hardware-efficient ansätze as an alternative, trading physical interpretability for reduced circuit depth.

---

## Limits / caveats

- **Expressibility vs. trainability:** The hardware-efficient ansatz has no physical motivation. It's unclear whether it can represent the ground state of larger molecules even at high depth. Subsequent work showed that deep HE ansätze suffer from barren plateaus — exponentially vanishing gradients.
- **LiH and BeH₂ not at chemical accuracy:** Despite 6-qubit circuits, the method fails chemical accuracy for BeH₂. This is partly hardware noise, partly insufficient expressibility at affordable depth.
- **Optimization:** SPSA is noisy and slow. Many iterations ($\sim 10^3$–$10^4$ shots per iteration $\times$ $10^2$–$10^3$ iterations) are needed. The total shot count can be enormous.
- **Readout-only error mitigation:** Only readout errors are corrected. Gate errors accumulate and are not mitigated. Subsequent work (zero-noise extrapolation, probabilistic error cancellation) addressed this.
- **Active space limitation:** The molecular Hamiltonians are restricted to minimal or small basis sets; absolute energies are far from converged CBS values.
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes|Barren plateaus]] — not recognized at the time of this paper, but hardware-efficient ansätze at large depth are now known to have exponentially vanishing gradients. This fundamentally limits scalability.

---

## Reusable ideas

1. [[Variational Quantum Eigensolver (VQE)]] — Standard VQE framework; this paper extends it to 6-qubit systems.

2. **Hardware-Efficient Ansatz** — Parameterized circuits designed to use native gates without decomposition. Allows deeper circuits at fixed gate count. The key insight: don't impose physical structure if hardware noise is the binding constraint.

3. [[Pauli Expectation Value Estimation]] — Energy measured as $H = \sum_k c_k P_k$; each Pauli measured with basis rotation.

4. [[Symmetry Reduction in Qubit Hamiltonians]] — Compact encoding reduces qubit count by 2 per symmetry sector.

5. [[Analytical Gradient Estimation for Variational Quantum Circuits]] — SPSA is used here as a noisy gradient estimator. Later work switched to parameter-shift rules.

---

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — VQE original proposal; uses photonic hardware and physics-motivated UCC ansatz.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — First VQE on superconducting hardware for H₂; this paper extends to larger molecules.
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]] — Theory paper establishing VQE framework.
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]] — Subsequent analysis showing that hardware-efficient ansätze at large depth have barren plateaus.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — UCC-based approach contrasted with HE-VQE here.
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Improved measurement strategies for VQE, including basis rotation grouping.
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]] — Error mitigation via subspace expansion, building on the noise analysis from this experiment.

### Trick cards
- [[Variational Quantum Eigensolver (VQE)]]
- [[Pauli Expectation Value Estimation]]
- [[Symmetry Reduction in Qubit Hamiltonians]]
- [[Bravyi-Kitaev Transformation]]
- [[Analytical Gradient Estimation for Variational Quantum Circuits]]
