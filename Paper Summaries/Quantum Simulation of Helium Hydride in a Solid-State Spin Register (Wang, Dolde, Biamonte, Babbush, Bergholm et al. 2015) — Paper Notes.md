> **Source:** Ya Wang, Florian Dolde, Jacob Biamonte, Ryan Babbush, Ville Bergholm, Sen Yang, Ingmar Jakobi, Philipp Neumann, Alán Aspuru-Guzik, James D. Whitfield, Jörg Wrachtrup, *Quantum Simulation of Helium Hydride Cation in a Solid-State Spin Register*, arXiv:1405.2696, ACS Nano **9**(8), 7769–7774 (2015)
> **Links:** [arXiv](https://arxiv.org/abs/1405.2696) · [Journal](https://doi.org/10.1021/acsnano.5b01651)
> **Tags:** #quantum-simulation #NV-center #diamond #quantum-chemistry #HeH+ #phase-estimation #solid-state #early-experiment

---

## The computational problem

Compute the bond dissociation curve of the helium hydride cation (HeH⁺) in the minimal STO-3G basis using a nitrogen-vacancy (NV) defect center in diamond as a quantum processor. The Hamiltonian after mapping is a 2-qubit operator, reducing to effectively 1 active qubit for the simplest version of the calculation.

HeH⁺ is the simplest heteronuclear diatomic molecule and is a standard proof-of-concept target for quantum chemistry experiments. In the minimal basis, the Hamiltonian has just one variational parameter (bond length $R$) and the configuration space is small enough to encode with 1–2 physical qubits.

---

## What the paper does

Wang et al. demonstrate quantum simulation of HeH⁺ using an NV center in diamond — the first quantum chemistry experiment on a solid-state spin register. The NV center provides:
- Two coupled spins: the nitrogen nuclear spin ($^{14}$N, spin-1) and the electron spin of the NV center (spin-1)
- Long coherence times at room temperature ($T_2 \sim$ ms for electron spin, seconds for nuclear spin)
- Single-qubit readout via photoluminescence

The experiment computes the bond dissociation curve of HeH⁺ using a phase estimation algorithm adapted for the NV center platform. Key result: energy uncertainty of order $10^{-14}$ Hartree — ten orders of magnitude below chemical accuracy — demonstrating that the NV platform can in principle achieve extreme precision.

This is notable as a demonstration that solid-state defect centers are a viable platform for quantum chemistry simulation, establishing a basis for later NV-center quantum computing research.

---

## The algorithm / construction

### Hamiltonian encoding for HeH⁺

HeH⁺ in the STO-3G basis has 2 spatial orbitals and 4 spin-orbitals. The ground state has 2 electrons. After Jordan-Wigner mapping and symmetry reduction (particle number = 2, spin sector selection), the Hamiltonian reduces to a 1-qubit or 2-qubit operator depending on the encoding choice.

The Hamiltonian takes the form:

$$H(\theta) = g_0(\theta) \mathbf{1} + g_1(\theta) Z + g_2(\theta) X$$

where the coefficients $g_k(\theta)$ are computed classically as a function of the bond angle parameter $\theta$ (proxy for bond length $R$).

### Configuration basis encoding

Unlike scalable approaches ([[Bravyi-Kitaev Transformation|BK]] or Jordan-Wigner in the second-quantized basis), the NV experiment uses a **configuration basis**: the two-dimensional Hilbert space spanned by the two Slater determinants $|\phi_1\rangle = |1_\uparrow 0_\downarrow\rangle$ and $|\phi_2\rangle = |0_\uparrow 1_\downarrow\rangle$ is directly encoded in the spin register.

This is not scalable (exponential preprocessing for larger molecules) but is efficient for 2-electron, 2-orbital systems.

### Phase estimation on NV center

The phase estimation protocol adapts Kitaev's IPEA to the NV spin system:

1. **Initialization:** Prepare the electron spin in the $|m_s = 0\rangle$ state via optical pumping
2. **Reference state:** Prepare the nuclear spin in a product state (easy to initialize)
3. **Controlled evolution:** Use the hyperfine interaction $A_{zz} S_z I_z$ as the coupling to implement controlled rotation operations
4. **Phase readout:** The electron spin phase encodes the energy; optical spin-state readout gives the phase bit

The nuclear spin serves as the register qubit; the electron spin serves as the ancilla. The coupled spin Hamiltonian of the NV center is:

$$H_{\rm NV} = D S_z^2 + \gamma_e B_z S_z + \gamma_n B_z I_z + A_{zz} S_z I_z + A_{\perp}(S_x I_x + S_y I_y)$$

The controlled-$e^{-iHt}$ operation is implemented by designing microwave pulse sequences that:
1. Select a specific spin transition
2. Apply a phase proportional to the molecular energy eigenvalue
3. Preserve coherence for the measurement

---

## Key results

- **Energy uncertainty:** $\delta E \sim 10^{-14}$ Hartree — 10 orders of magnitude below chemical accuracy within the chosen basis
- **Bond dissociation curve:** Computed at multiple bond lengths from 0.2–3 Å
- **Basis:** STO-3G minimal basis (not chemically converged)
- **Coherence:** Nuclear spin $T_2 \sim$ seconds allows many phase estimation steps

The $10^{-14}$ Ha precision is achievable because:
1. NV center nuclear spins have exceptionally long coherence times
2. Microwave pulses can be calibrated to very high precision
3. The 2-qubit system has no entanglement complexity — all "quantum" work is in phase accumulation

**It does not represent practical quantum advantage** — the same result is trivially obtained by exact classical diagonalization of the 2×2 Hamiltonian matrix. The value is as a platform demonstration.

---

## Comparison with prior work

| Platform | Paper | Molecule | Qubits | Approach | Chemical accuracy? |
|---|---|---|---|---|---|
| NMR | [[NMR Implementation of a Molecular Hydrogen Quantum Simulation with Adiabatic State Preparation (Du, Xu, Peng, Wang, Wu, Lu 2010) — Paper Notes|Du et al. 2010]] | H₂ | 2 | PEA + adiabatic | Yes |
| Photonic | [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes|Lanyon et al. 2010]] | H₂ | 2 | PEA | Yes |
| Photonic | [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. 2014]] | HeH⁺ | 2 | VQE | Yes |
| **NV center** | **Wang et al. (2015)** | **HeH⁺** | **1–2** | **PEA** | **Yes ($10^{-14}$ Ha)** |
| Superconducting | [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley et al. 2016]] | H₂ | 2 | VQE + PEA | VQE: Yes |

The NV-center experiment achieves the highest reported energy precision of any quantum chemistry experiment to date, enabled by nuclear spin coherence times.

---

## Limits / caveats

- **Classical triviality:** The 2×2 Hamiltonian is trivially diagonalizable on a laptop. No quantum advantage demonstrated.
- **Non-scalable encoding:** Configuration basis requires exponential classical preprocessing for larger molecules.
- **Minimal basis:** STO-3G results are not chemically converged. The bond dissociation curve is qualitatively correct but quantitatively inaccurate compared to complete basis set results.
- **No entanglement:** The NV-center experiment essentially uses the system as a phase accumulator; the "quantum simulation" is nearly classical for 2-qubit systems.
- **Platform constraints:** NV centers support only small spin registers (typically 1–5 qubits accessible with decent control); the platform cannot scale to classically intractable regimes without substantial engineering advances.

---

## Reusable ideas

1. [[Iterative Phase Estimation (Kitaev)]] — Adapted for the NV spin system using microwave pulse sequences and optical readout.

2. **Nuclear-Electronic Spin Register** — Using the NV electron spin as ancilla and the $^{14}$N nuclear spin as a long-lived quantum register. The nuclear spin's seconds-long coherence time enables high-precision phase accumulation.

3. [[Adiabatic State Preparation for Chemistry]] — Not used in this paper (initial state is a product state), but the paper references the adiabatic approach as an alternative state preparation method.

---

## Cross-links

### Paper notes
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — First VQE experiment; also on HeH⁺ in photonic hardware. Contemporaneous.
- [[NMR Implementation of a Molecular Hydrogen Quantum Simulation with Adiabatic State Preparation (Du, Xu, Peng, Wang, Wu, Lu 2010) — Paper Notes]] — NMR predecessor for H₂.
- [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes]] — Photonic experiment for H₂; same era.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — Uses scalable BK encoding; Table I explicitly compares against Wang et al. (2015) as an example of non-scalable configuration basis approach.
- [[Quantum Implementation of Unitary Coupled Cluster for Simulating Molecular Electronic Structure (Shen, Zhang, Chen, Zhang, Yung, Kim 2015) — Paper Notes]] — Ion-trap VQE experiment also targeting HeH⁺; different hardware and algorithm.

### Trick cards
- [[Iterative Phase Estimation (Kitaev)]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
