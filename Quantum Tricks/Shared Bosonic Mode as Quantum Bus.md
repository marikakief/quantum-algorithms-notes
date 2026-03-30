# Shared Bosonic Mode as Quantum Bus

> **Source:** Cirac and Zoller, PRL 74:4091, 1995
> **Tags:** #trick #quantum-hardware #two-qubit-gates #phonon #bosonic-mode #trapped-ions

## What it does
Mediates entangling two-qubit gates between qubits that don't directly interact, by temporarily encoding quantum information in a shared bosonic mode (phonon, photon, etc.) that couples to all qubits.

## The trick
The protocol has three steps:

1. **Map:** Transfer the quantum state of the control qubit onto the bus mode. For trapped ions, a red-sideband $\pi$-pulse on ion $m$ maps $\alpha|g\rangle + \beta|e\rangle$ onto phonon amplitudes $\alpha|0\rangle_{\text{ph}} + \beta|1\rangle_{\text{ph}}$ (leaving the ion in $|g\rangle$).

2. **Conditional operation:** Perform a gate between the bus mode and the target qubit. In the original Cirac-Zoller scheme, a $2\pi$ rotation through an [[Auxiliary Level for Conditional Phase Accumulation|auxiliary level]] gives a controlled-$Z$ between the phonon and ion $k$.

3. **Unmap:** Reverse Step 1, returning the bus to its ground state and restoring the control qubit.

The net effect is a controlled-phase (or CNOT) gate between ions $m$ and $k$, regardless of their positions in the chain.

The same pattern applies to other platforms:
- **Superconducting circuits:** Microwave cavity photon mediates transmon-transmon gates
- **Neutral atoms:** Shared photonic mode in a cavity (cavity QED)
- **NV centres:** Mechanical resonator or photonic bus

## When to reach for it
- Designing a two-qubit gate for qubits that interact indirectly via a shared mode
- Any situation where direct qubit-qubit coupling is weak but both qubits couple to a common bosonic field
- Extending qubit connectivity beyond nearest-neighbour without physical rearrangement

## Complexity
Requires three pulses for a CNOT. Gate time scales as $\sim 1/(\eta\Omega)$ where $\eta$ is the Lamb-Dicke parameter and $\Omega$ the Rabi frequency. The bus must start and end in a known state (usually the ground state), requiring preparation overhead.

## Caveat
The bus mode must be initialised in its ground state (or a known Fock state), which requires cooling. Motional heating during the gate degrades fidelity. The scheme uses only the $|0\rangle$ and $|1\rangle$ Fock states of the bus, so any population leakage to $|2\rangle$ or higher is an error. As the number of qubits grows, spectral crowding of bus modes makes this harder — modern ion-trap QC uses geometric phase gates (Mølmer-Sørensen) that are less sensitive to the motional state.

## Related notes
- [[Quantum Computations with Cold Trapped Ions (Cirac-Zoller 1995) — Paper Notes]]
- [[Auxiliary Level for Conditional Phase Accumulation]]
- [[Gate Teleportation Protocol]] — alternative approach for long-range entanglement
- [[A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001) — Paper Notes]] — photonic alternative where measurement replaces the bus
