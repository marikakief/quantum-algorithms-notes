> **Source:** Yangchao Shen, Xiang Zhang, Shuaining Zhang, Jing-Ning Zhang, Man-Hong Yung, Kihwan Kim, *Quantum implementation of the unitary coupled cluster for simulating molecular electronic structure*, arXiv:1506.00443, Phys. Rev. A **95**, 020501(R) (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1506.00443) · [Journal](https://doi.org/10.1103/PhysRevA.95.020501)
> **Tags:** #VQE #UCC #ion-trap #quantum-chemistry #HeH+ #variational #NISQ #early-experiment

---

## The computational problem

Simulate the ground state energy of the helium hydride cation (HeH⁺) using the unitary coupled cluster (UCC) ansatz in a trapped-ion quantum processor. Specifically: minimize $\langle \psi(\theta) | H | \psi(\theta) \rangle$ over the UCC parameter $\theta$, where $|\psi(\theta)\rangle = e^{T(\theta) - T^\dagger(\theta)} |\phi_{\rm HF}\rangle$ and $T(\theta)$ is the UCCS (singles) cluster operator.

Secondary goals: probe excited state energies (via parameter continuation), and simulate bond dissociation non-perturbatively (the interaction strength diverges at large bond length, defeating perturbative methods).

---

## What the paper does

This paper is the **first experimental demonstration of UCC-based variational quantum simulation** in a trapped-ion system. Using a 2-ion ($^{171}$Yb⁺) system, the authors implement the UCC ansatz for HeH⁺ and measure the ground-state energy at multiple bond lengths.

The paper provides experimental evidence that the UCC ansatz can be reliably implemented on quantum hardware, overcoming the "non-unitary nature of classical coupled cluster" that makes classical UCC computationally intractable. The key claim: quantum hardware naturally implements $e^{T - T^\dagger}$ without the need for the approximate Baker-Campbell-Hausdorff truncation required by classical algorithms.

The VQE+UCC result achieves sub-chemical-accuracy energies for HeH⁺ at all bond lengths. The excited state energies are probed by running VQE at modified parameter values (orthogonal subspace ansatz).

---

## The algorithm / construction

### HeH⁺ Hamiltonian

HeH⁺ in the minimal STO-3G basis has 2 spatial orbitals, 4 spin-orbitals, 2 electrons. After Jordan-Wigner mapping and symmetry reduction, the Hamiltonian maps to:

$$H = g_0 \mathbf{1} + g_1 Z_1 + g_2 Z_2 + g_3 Z_1 Z_2 + g_4 X_1 X_2 + g_5 Y_1 Y_2$$

with coefficients $g_k(R)$ computed classically. This is the same Hamiltonian form as in [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley et al.]] for H₂ and [[Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang, Dolde, Biamonte, Babbush, Bergholm et al. 2015) — Paper Notes|Wang et al.]] for HeH⁺.

### UCC ansatz

The UCCS (singles) ansatz for 2 electrons in 2 orbitals has a single variational parameter $\theta$:

$$|\psi(\theta)\rangle = e^{i\theta (a^\dagger_1 a_0 - a^\dagger_0 a_1)} |\phi_{\rm HF}\rangle$$

After Jordan-Wigner mapping, this becomes:

$$|\psi(\theta)\rangle = e^{i\theta (Y_0 X_1 - X_0 Y_1)/2} |\text{HF}\rangle$$

Implemented as:
$$|\psi(\theta)\rangle = R_{yy}(\theta) |\text{HF}\rangle$$

where $R_{yy}(\theta) = e^{i\theta Y \otimes X / 2}$ is a two-qubit entangling gate.

### Trapped-ion implementation

Two $^{171}$Yb⁺ ions are used, with:
- Qubit states: $|0\rangle, |1\rangle$ as hyperfine levels
- Two-qubit gate: Mølmer-Sørensen (MS) gate implementing $e^{i\theta XX/2}$ via laser-induced coupling
- Single-qubit rotations: resonant microwave or Raman pulses

The UCC circuit depth: 1 MS gate + single-qubit rotations. Total gate count: $\sim 5$ gates.

### Measurement and optimization

The energy is measured by decomposing $H$ into Pauli terms:
$$E(\theta) = g_0 + g_1 \langle Z_1 \rangle + g_2 \langle Z_2 \rangle + g_3 \langle Z_1 Z_2 \rangle + g_4 \langle X_1 X_2 \rangle + g_5 \langle Y_1 Y_2 \rangle$$

Each expectation value is measured by applying basis-change circuits before measurement.

**Optimization:** 1D scan over $\theta \in [-\pi, \pi]$ at each bond length (feasible for a single parameter). The minimum gives the ground-state energy estimate.

### Excited state probing

By running the VQE scan and identifying secondary local minima, excited-state energies are extracted. This is non-rigorous (no orthogonality guarantee) but gives qualitatively correct excited-state energies in this simple system.

---

## Key results

- **Ground state energy:** Below chemical accuracy ($< 10^{-3}$ Ha error) for all bond lengths from 0.2 to 3 Å
- **Bond dissociation:** Non-perturbative dissociation curve correctly recovered; classical coupled cluster (CCSD) fails at large $R$ but UCC succeeds
- **Excited states:** First and second excited states probed, in good agreement with exact diagonalization
- **Gate depth:** $\sim 5$ total gates for the UCC circuit — among the shallowest quantum chemistry circuits demonstrated

| Property | Value |
|---|---|
| Molecule | HeH⁺ |
| Basis | STO-3G |
| Qubits | 2 (2 ions) |
| Gate count | ~5 |
| Ground state error | $< 10^{-3}$ Ha |
| Chemical accuracy? | Yes (within basis) |

---

## Comparison with prior work

| Paper | Hardware | Molecule | Algorithm | Chemical accuracy? |
|---|---|---|---|---|
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. 2014]] | Photonic | HeH⁺ | VQE (first demo) | Yes |
| [[NMR Implementation of a Molecular Hydrogen Quantum Simulation with Adiabatic State Preparation (Du, Xu, Peng, Wang, Wu, Lu 2010) — Paper Notes|Du et al. 2010]] | NMR | H₂ | PEA + adiabatic | Yes |
| [[Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang, Dolde, Biamonte, Babbush, Bergholm et al. 2015) — Paper Notes|Wang et al. 2015]] | NV center | HeH⁺ | PEA | Yes ($10^{-14}$ Ha) |
| **Shen et al. (2015)** | **Ion trap** | **HeH⁺** | **VQE + UCC** | **Yes** |
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley et al. 2016]] | Superconducting | H₂ | VQE + UCC (BK) | Yes (VQE) |

The key distinction from [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al.]] is the ion-trap platform and the explicit UCC ansatz (with a single exponential implementation). Peruzzo et al. used a linearized UCC (Trotterized product of exponentials), while this paper implements the exact UCC exponential in a single gate.

---

## Limits / caveats

- **2-electron, 2-orbital system:** Completely classically trivial. The "quantum advantage" is demonstrative only.
- **Non-scalable encoding:** Configuration basis; same limitation as all papers in this era.
- **HeH⁺ only:** No attempt to go beyond 2 electrons or 2 orbitals.
- **UCCS only:** The singles operator is the only excitation; for larger systems, UCC requires doubles, triples, etc., making the circuit exponentially deeper in general.
- **Excited states without orthogonality:** The excited-state extraction is heuristic; rigorous excited-state VQE requires more sophisticated approaches (e.g., subspace expansion, VQD).
- **Minimal basis:** STO-3G is not chemically converged.

---

## Reusable ideas

1. [[Unitary Coupled Cluster (UCC) Ansatz]] — The key conceptual contribution: quantum computers can implement $e^{T-T^\dagger}$ exactly in a single gate, overcoming classical UCC's intractability. For a single-parameter UCCS, this reduces to a two-qubit entangling gate.

2. [[Variational Quantum Eigensolver (VQE)]] — VQE framework; energy minimization via classical outer loop over quantum circuit parameters.

3. [[Pauli Expectation Value Estimation]] — Measure each Pauli term of the Hamiltonian separately via basis-change rotations.

4. **Non-perturbative Bond Dissociation** — UCC correctly describes bond dissociation in the strongly correlated regime where perturbative classical methods (like Hartree-Fock) fail. The variational principle guarantees an upper bound on the ground-state energy at all geometries.

---

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — First VQE experiment; inspiration for this work. Photonic vs. ion trap.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — Theory paper developing UCC scaling strategies; builds on the circuit design demonstrated here.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — Superconducting VQE+UCC for H₂ with scalable BK encoding.
- [[Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang, Dolde, Biamonte, Babbush, Bergholm et al. 2015) — Paper Notes]] — NV-center PEA experiment for HeH⁺; different algorithm (PEA vs VQE), same molecule.
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]] — Theory paper published concurrently; provides the formal analysis of VQE.

### Trick cards
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Pauli Expectation Value Estimation]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
