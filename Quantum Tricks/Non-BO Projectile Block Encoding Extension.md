
> **Source:** Rubin, Berry, Kononov, Malone, Khattar, White, Lee, Neven, Babbush, Baczewski, arXiv:2308.12352
> **Tags:** #trick #block-encoding #first-quantized #non-Born-Oppenheimer #qubitization #plane-wave

## What it does

Extends a [[First-Quantized Plane-Wave Chemistry Encoding|first-quantized plane-wave]] block encoding to include a quantum projectile nucleus with a different (larger) momentum grid than the electrons, without restructuring the block encoding.

## The trick

Starting from the [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]] block encoding of $H = T + U + V$, add three new terms: projectile kinetic energy $T_{\text{proj}}$ (in the shifted momentum frame $k_p - k_{\text{proj}}$), projectile-nuclei potential $U_{\text{proj}}$, and electron-projectile potential $V_{\text{proj}}$.

The key insight: $U_{\text{proj}}$ requires a $1/\|\nu\|$ superposition over a larger set $\tilde{G}_0$ (differences of projectile momenta), while all other potential terms use the smaller $G_0$ (electron momentum differences). Rather than building two separate PREPARE circuits, **make the existing [[Nested Box State Preparation with Overlapping Boxes|nested box preparation]] controlled**: the initial cascading Hadamard sequence for preparing the unary-encoded $|\mu\rangle$ state gains $n_n - n_p$ additional controlled Hadamards, toggled by a single qubit selecting $U_{\text{proj}}$ vs. everything else. Cost: $n_n - n_p$ extra Toffolis (typically 2–3). The rest of the preparation proceeds identically.

For the momentum cross-term $T_{\text{mean}} = -\sum_w k_w^{\text{proj}} k_w^p / M_{\text{proj}}$, represent $k_{\text{proj}}$ as all-ones in a register with the actual value absorbed into the state preparation amplitudes. SELECT distinguishes $p^2$ from $p \cdot p_{\text{proj}}$ via a single AND gate controlled by a flag qubit selecting $T_{\text{mean}}$.

The [[Failed PREP Fallback to Alternative Hamiltonian Term|failed PREP fallback]] extends naturally: on state preparation failure, a three-outcome register selects among $T_{\text{elec}}$, $T_{\text{proj}}$, $T_{\text{mean}}$ via two inequality tests (cost: $\sim 6n_T + 2$ Toffolis total).

## When to reach for it

- Any [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based simulation where one particle species needs a larger basis (more qubits per register) than another
- Non-BO simulations where the projectile/impurity has very different momentum requirements from the bath particles
- Mixed particle-species simulations (e.g., electron-muon, electron-positron) in first quantization

## Complexity

Dominant cost increase: $6n_n$ Toffolis for the [[Controlled Swap Network for Select Oracle|controlled swaps]] of the projectile register into the working register (vs. $12\eta n_p$ for all electrons). This is subdominant for $\eta \gg 1$.

State preparation overhead: $n_n - n_p$ Toffolis (controlled Hadamards) + $2(n_n - n_p)$ for the momentum bit preparation + 16 for the $w$-register inequality tests + 4 for controlled swaps between kinetic sub-selections.

## Caveat

The controlled $\nu$ preparation shares a register between electron and projectile momentum differences. This requires $n_n \geq n_p$ — if the projectile needed fewer qubits than the electrons, you'd pad the projectile register and waste some amplitude.

## Related notes
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]]
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]
- [[Failed PREP Fallback to Alternative Hamiltonian Term]]
- [[First-Quantized Plane-Wave Chemistry Encoding]]
- [[Controlled Swap Network for Select Oracle]]
- [[Nested Box State Preparation with Overlapping Boxes]]
- [[Kinetic Energy as Bit-Product LCU]]
