# Byproduct Propagation in MBQC

> **Source:** Raussendorf & Briegel, arXiv:quant-ph/0010033
> **Tags:** #trick #MBQC #Pauli #byproduct #feed-forward

## What it does
Handles the random Pauli operators that arise from measurement outcomes in measurement-based quantum computation, by algebraically propagating them through the circuit to the output where they can be corrected classically.

## The trick
Each measurement in MBQC produces a random outcome, resulting in an unwanted Pauli byproduct ($\sigma_x$, $\sigma_z$, or $\sigma_x\sigma_z$) on the logical qubit. Instead of correcting immediately, propagate the byproduct forward through subsequent gates using commutation relations:

$$\mathrm{CNOT}\,(\sigma_x \otimes I) = (\sigma_x \otimes \sigma_x)\,\mathrm{CNOT}$$
$$\mathrm{CNOT}\,(I \otimes \sigma_z) = (\sigma_z \otimes \sigma_z)\,\mathrm{CNOT}$$
$$U_x(\zeta)U_z(\eta)U_x(\xi)\,\sigma_z = \sigma_z\,U_x(-\zeta)U_z(\eta)U_x(-\xi)$$

The Pauli operators transform as they pass through each gate — they may change type or spread to other wires — but they remain Pauli operators throughout. At the output, the accumulated byproduct is a known Pauli, correctable by adjusting the final measurement basis.

The sign-flip in the third relation is why measurement bases must be adaptive: commuting $\sigma_x$ past a rotation flips the sign of two Euler angles, which must be compensated by choosing the opposite measurement angle.

## When to reach for it
- Any computation model where random Pauli errors accumulate and need tracking (MBQC, [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes|gate teleportation]], Clifford+T circuits with random Pauli frames)
- Pauli frame tracking in fault-tolerant quantum computation
- Randomised compiling schemes where Pauli twirling is used and outcomes must be untwirled

## Complexity
Tracking byproducts costs 2 classical bits per logical wire, updated $O(1)$ per gate. The overhead is purely classical.

## Caveat
Works because the universal gate set generates the Clifford group under Pauli conjugation (Paulis map to Paulis through Cliffords). For non-Clifford gates (rotations), the byproduct propagation changes the gate parameters — this is what forces the adaptive measurement schedule and creates a partial temporal ordering on measurements.

## Related notes
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]]
- [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes]]
- [[Adaptive Measurement Bases for Feed-Forward]]
- [[Cluster State as Universal Entanglement Resource]]
