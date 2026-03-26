# Hybrid Classical-Quantum Chemistry Workflow

> **Source:** Reiher, Wiebe, Svore, Wecker, Troyer, arXiv:1605.03590
> **Tags:** #trick #quantum-chemistry #resource-estimation #workflow #fault-tolerant

## What it does
Decomposes computational chemistry into classical and quantum stages, using the quantum computer only for the step that is classically intractable — ground-state energy evaluation in a large active space.

## The trick
The workflow has six stages, of which only one runs on a quantum computer:

1. **Structure optimization** (classical): DFT for molecular geometries. Quantum computers don't help here — DFT structures are reliable even when DFT energies are not.
2. **Orbital selection** (classical): small-CAS CASSCF or UHF natural orbitals to define the active space. Automated via Pulay's UNO-CAS criterion or similar.
3. **Integral transformation** (classical): compute $h_{pq}$ and $h_{pqrs}$ in the molecular orbital basis. Standard four-index transformation, $O(N^5)$ classically.
4. **Ground-state energy** (**quantum**): [[Quantum Phase Estimation|QPE]] on the second-quantized Hamiltonian in the active space. This is the bottleneck — the exponential cost that motivates the quantum computer.
5. **Thermochemistry corrections** (classical): vibrational frequencies, temperature, entropy from DFT Hessians. Standard protocols (rigid rotor / harmonic oscillator).
6. **Kinetic modeling** (classical): energy differences enter Eyring absolute rate theory for reaction rate constants.

The quantum computer acts as an accelerator — a coprocessor for step 4 embedded in an otherwise classical pipeline.

## When to reach for it
Any time you're doing quantum chemistry resource estimation. This decomposition clarifies which costs are quantum (and scale with active space size) and which are classical (and can be done with standard methods). It applies to:
- Enzyme mechanism elucidation (the original FeMoCo application)
- Catalysis design (transition-metal complexes)
- Materials science (when combined with [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Bloch orbital]] or [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|first-quantized]] approaches)

## Complexity
The quantum cost is entirely in step 4. The classical costs are polynomial and well-understood. The total end-to-end cost is dominated by QPE, which scales as $O(\lambda/\varepsilon)$ for [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based approaches or $O(N^{10}t^2/\varepsilon)$ for the original Trotter approach in this paper.

## Caveat
The workflow assumes you can define a meaningful active space that captures the relevant chemistry. For strongly correlated systems spanning many orbitals, active space selection itself is an art. Also, dynamic correlation beyond the active space must be recovered classically (via perturbation theory or range-separated DFT) — the paper discusses this but doesn't resolve it.

## Related notes
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]]
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes]]
- [[Error Budget Optimization for Fault-Tolerant Algorithms]]
- [[Classical-Quantum Runtime Crossover Analysis]]
