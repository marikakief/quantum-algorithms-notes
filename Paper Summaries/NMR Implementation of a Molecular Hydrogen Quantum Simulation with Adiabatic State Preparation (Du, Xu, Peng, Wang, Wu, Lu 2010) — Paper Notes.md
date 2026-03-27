> **Source:** Jiangfeng Du, Nanyang Xu, Xinhua Peng, Pengfei Wang, Sanfeng Wu, Dawei Lu, *NMR implementation of a molecular hydrogen quantum simulation with adiabatic state preparation*, Phys. Rev. Lett. **104**, 030502 (2010)
> **Links:** [Journal](https://doi.org/10.1103/PhysRevLett.104.030502)
> **Tags:** #quantum-simulation #NMR #quantum-chemistry #H2 #phase-estimation #adiabatic-state-preparation #molecular-energy #early-experiment

---

## The computational problem

Compute the ground-state energy of molecular hydrogen (H₂) in the STO-3G minimal basis as a function of internuclear distance. The second-quantized Hamiltonian has 4 spin-orbitals; after symmetry reduction it maps to a 2-qubit Hamiltonian. Target precision: sufficient to validate that quantum simulation correctly recovers the potential energy surface near equilibrium.

Formally: given the molecular Hamiltonian $H = \sum_{pq} h_{pq} a^\dagger_p a_q + \frac{1}{2}\sum_{pqrs} h_{pqrs} a^\dagger_p a^\dagger_q a_r a_s$ for H₂, estimate the ground state energy $E_0(R)$ as a function of bond length $R$.

---

## What the paper does

This is one of the earliest experimental demonstrations of quantum simulation of a molecular system. Using a 4-qubit NMR quantum processor (based on a $^{13}$C-labeled alanine molecule), Du et al. compute the H₂ energy surface by:

1. **Adiabatic state preparation:** Prepare the ground state of a simple reference Hamiltonian and adiabatically evolve to the H₂ Hamiltonian
2. **Iterative phase estimation:** Use an NMR-adapted iterative phase estimation algorithm (NMR interferometer) to extract the ground state energy from the phase of a controlled time evolution

The key result: 45-bit estimation of the energy value, achieving far beyond chemical accuracy within the chosen basis set. The experiment achieves energy error $\sim 10^{-5}$ Hartree — far below the $\sim 10^{-3}$ Hartree chemical accuracy threshold.

This paper appeared roughly contemporaneously with [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes|Lanyon et al. 2010]] (photonic) and contributed to establishing NMR as a platform for early proof-of-concept quantum chemistry simulations.

---

## The algorithm / construction

### Hamiltonian encoding

H₂ in the STO-3G basis has 4 spin-orbitals. After Jordan-Wigner mapping and symmetry reduction (particle number and spin-$z$ conservation), the Hamiltonian acts on 2 qubits:

$$H = g_0 \mathbf{1} + g_1 Z_1 + g_2 Z_2 + g_3 Z_1 Z_2 + g_4 X_1 X_2 + g_5 Y_1 Y_2$$

where the coefficients $g_k(R)$ are computed classically from the molecular integrals at each bond length $R$.

In the NMR encoding, this 2-qubit Hamiltonian is embedded in the 4-spin NMR register via a configuration-basis representation. The energy eigenstates of the molecular Hamiltonian correspond to specific nuclear spin states in the NMR system.

### Adiabatic state preparation

The experiment uses a **discrete adiabatic state preparation** protocol:

1. Start in the ground state of a reference Hamiltonian $H_{\rm ref}$ (a simple diagonal term), which is easy to prepare in NMR
2. Apply a sequence of small Trotter steps interpolating between $H_{\rm ref}$ and $H_{\rm mol}$:
$$H(s) = (1-s) H_{\rm ref} + s H_{\rm mol}, \quad s = 0 \to 1$$
3. If the evolution is adiabatic (slow enough relative to the gap), the system remains in the ground state

The adiabatic path is discretized into $\sim 100$ steps, each implemented as an NMR pulse sequence.

### Iterative phase estimation (NMR interferometer)

After preparing $|\psi_0\rangle \approx |E_0\rangle$, the energy is extracted via an iterative phase estimation protocol adapted for NMR:

1. Apply Hadamard to ancilla spin
2. Apply controlled-$e^{-iH_{\rm mol} t_k}$ for time $t_k = 2^k t_0$
3. Apply classical phase correction and Hadamard
4. Measure ancilla
5. Repeat with feedback (Kitaev's IPEA protocol)

The NMR implementation uses an **interferometric sequence** in which the ancilla spin's phase accumulates the eigenphase:

$$\langle \sigma_+^{\rm anc} \rangle = \frac{1}{2}\text{Re}[e^{i E_0 t_k}]$$

By measuring this for different $t_k$ and using classical postprocessing, the energy is extracted to 45 bits of precision.

**Why 45 bits?** The total circuit time is bounded by the NMR coherence time ($T_2 \sim$ seconds), so the maximum phase accumulation time is fixed. 45 bits of precision is achieved by using different time scales and classical combination.

---

## Key results

- **Ground state energy at $R = 0.74$ Å (equilibrium):** Measured to $\sim 10^{-5}$ Ha precision
- **45-bit phase estimation:** Enabled by NMR's long coherence times ($T_2 \sim$ seconds) compared to other platforms
- **Dissociation curve:** Energy computed at multiple bond lengths, recovering the H₂ potential energy surface
- **Error:** Dominated by systematic Hamiltonian simulation error (Trotter error), not quantum noise — NMR is a low-noise platform for proof-of-concept

**Key comparison:**

| Platform | Paper | Molecule | Bits of energy precision | Chemical accuracy? |
|---|---|---|---|---|
| NMR | Du et al. (2010) | H₂ | 45 | Yes (within basis) |
| Photonic | Lanyon et al. (2010) | H₂ | ~10 | Yes |
| Photonic | Peruzzo et al. (2014) | HeH⁺ | ~15 | Yes |
| Superconducting | O'Malley et al. (2016) | H₂ | ~10 | Yes (VQE) |

---

## Comparison with prior work

The only comparable experiment at the time was Aspuru-Guzik et al.'s **classical simulation** proposal (2005) and the contemporaneous **Lanyon et al. (2010)** photonic experiment. The NMR paper demonstrates:

- **Adiabatic state preparation as an alternative to Hartree-Fock initialization:** Instead of assuming the initial state is close to HF, use quantum adiabatic evolution to prepare the ground state from a trivial reference
- **High-precision phase estimation:** NMR's long coherence times allow accumulating far more phase than photonic or superconducting experiments of the era
- **Configuration-basis encoding:** Uses a non-scalable encoding (exponentially many circuits for larger molecules) — the scalable Bravyi-Kitaev approach was introduced in [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley et al. 2016]]

---

## Limits / caveats

- **Non-scalable encoding:** The configuration-basis representation requires exponential classical preprocessing to construct controlled-$U$ operations. Scaling to larger molecules requires exponentially growing circuit compilations.
- **H₂ only:** The result is for the minimal molecule in the minimal basis. No path to larger systems is demonstrated.
- **NMR coherence ≠ fault-tolerance:** NMR experiments on ensemble systems benefit from very long natural coherence times, but the quantum computation is an analog simulation that would not generalize to universal error-corrected quantum computers without substantial redesign.
- **Energy precision vs. chemical precision:** The 45-bit precision is impressive, but it's within the chosen basis (STO-3G), which is far from the complete basis set limit. The "chemical accuracy" claim is within the basis, not for the real molecule.
- **No demonstration beyond H₂:** Unlike [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes|Lanyon et al.]] which also studied H₂, no larger molecule is attempted.

---

## Reusable ideas

1. [[Adiabatic State Preparation for Chemistry]] — Use a discrete adiabatic evolution from a simple reference Hamiltonian to the target Hamiltonian as an alternative to HF initialization. Useful when the HF state has poor overlap with the ground state (strongly correlated systems).

2. [[Iterative Phase Estimation (Kitaev)]] — Kitaev's IPEA using a single ancilla with classical feedback. The NMR implementation is a physical adaptation of this algorithm using spin-echo sequences to implement controlled evolution.

3. [[IPEA with Constant-Depth Controlled Powers]] — In NMR, different $t_k$ are achieved by varying the pulse length; the ancilla spin accumulates phase from controlled-$e^{-iHt_k}$ in a Ramsey-type sequence.

---

## Cross-links

### Paper notes
- [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes]] — Contemporaneous photonic experiment for H₂; different hardware, similar result, non-scalable encoding.
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]] — Original theory proposal for quantum chemistry QPE; this paper is one of the first experiments.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — Successor experiment on superconducting hardware using scalable Bravyi-Kitaev encoding; compares against Du et al. explicitly.
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — Photonic VQE experiment; contrasts with the QPE approach here.
- [[Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang, Dolde, Biamonte, Babbush, Bergholm et al. 2015) — Paper Notes]] — NV-center experiment for HeH⁺; same era, different platform.

### Trick cards
- [[Adiabatic State Preparation for Chemistry]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Trotterized Time Evolution for Chemistry]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
