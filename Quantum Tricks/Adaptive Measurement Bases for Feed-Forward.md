# Adaptive Measurement Bases for Feed-Forward

> **Source:** Raussendorf & Briegel, arXiv:quant-ph/0010033
> **Tags:** #trick #MBQC #adaptive #feed-forward #measurement

## What it does
Makes measurement-based computation deterministic despite the randomness of individual measurement outcomes, by conditioning each measurement basis on the results of previous measurements.

## The trick
In MBQC, implementing a rotation $U_R(\xi, \eta, \zeta) = U_x(\zeta)U_z(\eta)U_x(\xi)$ on a chain of 5 cluster qubits requires measuring qubits 2, 3, 4 at angles $\xi$, $\eta$, $\zeta$ in the $x$-$y$ plane. But the sign of each angle depends on earlier measurement outcomes because of [[Byproduct Propagation in MBQC|byproduct propagation]]: commuting a $\sigma_x$ byproduct past a rotation flips two of three Euler angles.

Concretely, if the measurement outcome at qubit $j$ flips the byproduct, the angle for qubit $j+1$ is negated. The rule is simple:
- Track a 1-bit "parity" of byproducts from all earlier measurements on that wire
- If parity is odd, negate the measurement angle; if even, keep it

This creates a **partial temporal ordering**: measurements within a single gate must be sequential (each depends on the previous outcome), but measurements on independent wires can proceed in parallel.

## When to reach for it
- Any protocol where random outcomes affect future operations and you need deterministic overall behaviour
- Blind quantum computation: the client sends adapted angles to the server without revealing the computation
- Feed-forward in photonic quantum computing, where measurement outcomes trigger real-time basis rotations

## Complexity
One classical bit of feed-forward per measurement, with $O(1)$ classical processing per step. The sequential depth within a single gate is equal to the number of measurements in that gate (e.g., 4 for an arbitrary rotation).

## Caveat
The feed-forward requirement prevents full parallelisation of measurements. The circuit depth (in measurement rounds) is at least the depth of the simulated circuit. In photonic implementations, the speed of classical feed-forward electronics is the bottleneck.

## Related notes
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]]
- [[Byproduct Propagation in MBQC]]
- [[Measurement-Driven Computation via Stabiliser Relations]]
- [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes]]
