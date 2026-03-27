> **Source:** Frank Arute, Kunal Arya, Ryan Babbush, et al. (Google AI), *Observation of separated dynamics of charge and spin in the Fermi-Hubbard model*, arXiv:2010.07965 (2020). Preprint; see also companion results in related Google Quantum AI publications.
> **Links:** [arXiv](https://arxiv.org/abs/2010.07965)
> **Tags:** #quantum-simulation #Fermi-Hubbard #superconducting #Sycamore #spin-charge-separation #NISQ #Floquet #time-evolution #free-fermion

---

## The computational problem

Simulate the real-time dynamics of the Fermi-Hubbard model on a one-dimensional chain:

$$H_{\rm FH} = -t \sum_{\langle i,j \rangle, \sigma} (a^\dagger_{i\sigma} a_{j\sigma} + \text{h.c.}) + U \sum_i n_{i\uparrow} n_{i\downarrow}$$

Specifically, study the phenomenon of **spin-charge separation**: in 1D, charge and spin degrees of freedom propagate at different velocities, a hallmark of Luttinger liquid physics. The experiment focuses on observing this separation in a chain of $\sim$12 fermionic sites using the Sycamore quantum processor.

The experimentally studied regime is effectively the **free-fermion limit** ($U = 0$ of the Hubbard model) or weakly-interacting regime, where Trotterized dynamics can be validated classically and hardware performance assessed.

---

## What the paper does

This paper demonstrates the first quantum simulation of non-trivial fermionic dynamics — specifically the separation of charge and spin propagation velocities — on a programmable quantum processor. Using Google's Sycamore chip with $\sim 12$ active qubits and up to $\sim 500$ two-qubit gates, the experiment measures spreading of local observables (charge density, spin density) after a quench from a product initial state.

The key innovation is using **Floquet circuits** as the simulation primitive rather than conventional microwave-gate Trotter steps. The Floquet approach compiles the Trotter step into the hardware's native parametric coupling, eliminating decomposition overhead and drastically reducing gate depth. This allows more Trotter steps before decoherence kills the signal.

The results are compared against exact classical simulation (tractable since the free-fermion limit is classically solvable) to validate the quantum hardware — a necessary calibration step before claiming quantum advantage in the interacting regime.

---

## The algorithm / construction

### Floquet simulation

Instead of decomposing $e^{-iHt/r}$ into elementary gates via standard Trotter decomposition, the Sycamore processor is tuned to directly implement a Trotter step as a single Floquet cycle. The native parametric coupling between adjacent qubits (frequency-tunable transmons) is used to implement:

$$U_{\rm hop}(t) = e^{-it \cdot t_{\rm hopping} \cdot (a^\dagger_i a_j + a^\dagger_j a_i)}$$

for both spin-up and spin-down channels in a single cycle of the Floquet drive. This corresponds to a first-order Trotter step for the hopping term.

The on-site interaction $e^{-iU n_{i\uparrow} n_{i\downarrow} \delta t}$ is implemented via a controlled-phase gate calibrated to the Hubbard $U$ parameter.

### Initial state and observables

Initial state: a Néel-like product state with alternating site occupancies, prepared deterministically.

Observables measured:
- **Charge density** $\langle n_{i\uparrow} + n_{i\downarrow} \rangle$ as a function of time and position
- **Spin density** $\langle n_{i\uparrow} - n_{i\downarrow} \rangle$ as a function of time and position

Spin-charge separation manifests as: charge density spreads at velocity $v_c$, spin density spreads at velocity $v_s < v_c$, with $v_s/v_c$ depending on interaction strength.

### Free-fermion limit and validation

The free-fermion ($U = 0$) case serves as a calibration reference because:
1. Exact classical simulation is tractable via matchgate simulation (free-fermion formalism)
2. The spin and charge velocities are equal ($v_s = v_c = 2t_{\rm hop}/\hbar$ in this limit)
3. Agreement between quantum experiment and classical simulation quantifies hardware error

The interacting case ($U \neq 0$) shows spin-charge separation, and the velocities are measured from the wavefront of spreading in the space-time density plots.

### Gate counts and circuit depth

| Parameter | Value |
|---|---|
| Active qubits | ~12 |
| Two-qubit gates per Trotter step | ~24 (linear chain, 2 spins) |
| Total Trotter steps | ~5–20 |
| Total two-qubit gates | ~120–480 |
| Dominant noise source | Gate error, decoherence |

---

## Key results

**Experimental observation:**
- Clear separation of charge and spin wavefronts observed in the Fermi-Hubbard chain
- Charge velocity $v_c > v_s$ (spin velocity) at interaction strength $U > 0$
- Both velocities extracted from time-resolved density profiles

**Calibration (free-fermion case):**
- Agreement between quantum hardware and exact classical simulation to within error bars
- Demonstrates that Floquet-based Trotter simulation is more accurate than microwave-gate-based approaches at the same circuit depth

**Hardware benchmark:**
- Up to ~500 two-qubit gates executed with sufficient fidelity to extract qualitative physics
- Error mitigation: postselection on particle number conservation

**Scientific claim:**
- This is, at the time, the largest-scale quantum simulation of non-trivial fermionic dynamics that could not be directly validated by brute-force classical simulation (though the specific regime remains classically tractable via specialized algorithms)

---

## Comparison with prior work

| Paper | Hardware | System | Algorithm | Two-qubit gates | Classically tractable? |
|---|---|---|---|---|---|
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley et al. 2016]] | Superconducting | H₂ | VQE + PEA | 2–14 | Yes |
| [[Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala, Mezzacapo, Temme, Takita, Brink, Chow, Gambetta 2017) — Paper Notes|Kandala et al. 2017]] | Superconducting | BeH₂, Ising | HE-VQE | ~50 | Yes |
| **Arute et al. 2020 (FH)** | **Sycamore** | **Fermi-Hubbard** | **Floquet Trotter** | **~500** | **Yes (free fermion limit)** |
| [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes|Tazhigulov et al. 2022]] | Sycamore | Fe-S, RuCl₃ | QITE + recomp. | ~310 | Some regimes |

The key advance is the demonstration of Floquet-based simulation as a practical technique for condensed matter physics on superconducting hardware.

---

## Limits / caveats

- **Free-fermion limit is classically easy:** The regime where spin-charge separation is most clearly observable and most quantum-hardware-accessible (weak to moderate $U$) is also classically simulable via free-fermion methods. Genuine quantum advantage requires the strongly-correlated regime, which requires more qubits and gates.
- **1D chain:** Real Mott insulators and Luttinger liquids require 2D geometries or longer chains; 12 sites is well within classical reach.
- **Error accumulation:** At ~500 two-qubit gates, errors are non-negligible. The paper uses postselection (number conservation) but not zero-noise extrapolation or other error mitigation for the central result.
- **Floquet-specific:** The Floquet trick works beautifully for the Fermi-Hubbard model on a linear chain where hopping and interaction terms align with the native hardware; less general for other Hamiltonians.
- **Preprint only:** The arXiv:2010.07965 paper remained a preprint as of writing; it was not published in a peer-reviewed journal.

---

## Reusable ideas

1. [[Floquet Gate Calibration for Circuit Recompilation]] — Compile Trotter steps directly into parametric Floquet drives, eliminating gate decomposition overhead. Reduces effective circuit depth by $3$–$5\times$ over standard decomposition.

2. [[Fermionic Swap Network]] — Implementing fermionic hopping on a linear chain without SWAP overhead by using the Jordan-Wigner string structure of the Fermi-Hubbard Hamiltonian.

3. **Postselection for Error Mitigation** — Discard shots violating conserved quantities (particle number, spin parity) as a cheap error filter. Works when the conserved quantity is easily measured and the error rate for number-violating errors is significant.

4. [[Product-Formula Time-Slicing for Local Hamiltonians]] — Trotter decomposition for the Fermi-Hubbard model using Jordan-Wigner mapping.

---

## Cross-links

### Paper notes
- [[Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala, Mezzacapo, Temme, Takita, Brink, Chow, Gambetta 2017) — Paper Notes]] — Predecessor NISQ experiment; this paper scales up to more complex dynamics.
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]] — Successor Sycamore experiment targeting correlated chemistry via QITE + recompilation.
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — Fault-tolerant Trotter-based Fermi-Hubbard simulation analysis.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — Linear-depth Trotter circuits; the Floquet approach here achieves similar goals via hardware co-design.
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — Theoretical analysis of Trotter errors for local systems.

### Trick cards
- [[Floquet Gate Calibration for Circuit Recompilation]]
- [[Fermionic Swap Network]]
- [[Trotterized Time Evolution for Chemistry]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
