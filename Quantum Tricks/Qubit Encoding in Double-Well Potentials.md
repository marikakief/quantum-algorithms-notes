# Qubit Encoding in Double-Well Potentials

> **Source:** Jordan, Krovi, Lee, Preskill, arXiv:1703.00454
> **Tags:** #trick #qubit-encoding #quantum-field-theory #double-well #BQP-hardness

## What it does
Encodes a qubit using a single particle in a double-well potential, where the two logical states correspond to the ground and first excited states (or equivalently, particle localised in left vs right well).

## The trick
In a massive $\phi^4$ theory, the source term $J_2(t,x)\phi^2$ creates an effective nonrelativistic potential $V(x) = J_2(x)/m$. Shaping $J_2$ as a double well confines a single particle with two nearly-degenerate low-energy states:

- $|0_L\rangle = \frac{1}{\sqrt{2}}(|\psi_{\text{left}}\rangle + |\psi_{\text{right}}\rangle)$ (symmetric, ground state)
- $|1_L\rangle = \frac{1}{\sqrt{2}}(|\psi_{\text{left}}\rangle - |\psi_{\text{right}}\rangle)$ (antisymmetric, first excited)

The energy splitting between these states is exponentially small in the well separation $\ell$ (scales as $e^{-\ell\sqrt{2m V}}$), giving the qubit an exponentially long coherence time against tunnelling errors.

The inter-particle $\phi^4$ interaction provides two-qubit coupling: bringing wells from different qubits close together generates a contact interaction $\frac{\lambda}{4m^2}\delta(x_i - x_j)$ plus an $O(\lambda^2)$ Yukawa tail.

## When to reach for it
- Constructing BQP-hardness proofs for physical systems by embedding quantum computation into their dynamics
- Any setting where particles in potential wells (atoms in optical lattices, electrons in quantum dots) need to be formally shown to support universal quantum computation
- Designing qubit encodings that rely on spatial separation for error suppression rather than active error correction

## Complexity
State preparation: $O(n^8)$ for $n$ qubits (via adiabatic passage). Single-qubit gates: $O(\text{poly}(\log n))$ (tunnel through barrier). Two-qubit gates: $O(\lambda^{-2})$ per gate (adiabatic evolution in the 6D two-particle subspace).

## Caveat
Two-qubit gates via direct interaction are problematic: tunnelling between wells from different qubits is parametrically larger than the interaction energy when $\lambda$ is small. The fix requires working in a larger (6D) subspace of configurations without double occupancy, then projecting back. This makes the gate construction more complex than the encoding suggests. Also, $\lambda = O(1/G)$ means the coupling weakens with circuit size.

## Related notes
- [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes]]
- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes]]
- [[Adiabatic Passage for Particle Creation from Vacuum]]
- [[Leakage Suppression via Adiabatic Gate Design]]
