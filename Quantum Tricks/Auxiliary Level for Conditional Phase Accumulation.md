# Auxiliary Level for Conditional Phase Accumulation

> **Source:** Cirac and Zoller, PRL 74:4091, 1995
> **Tags:** #trick #quantum-hardware #controlled-phase #AMO #trapped-ions

## What it does
Implements a controlled-phase gate by driving a $2\pi$ rotation through a third atomic level outside the computational subspace, acquiring a geometric phase of $-1$ only when both the control condition (bus occupation) and the target qubit state are simultaneously satisfied.

## The trick
The computational qubit lives in $\{|g\rangle, |e\rangle\}$, and there exists an auxiliary level $|aux\rangle$ that can be coupled to $|e\rangle$ via a sideband transition involving the bus mode. A laser pulse drives the transition:

$$|e\rangle_k |1\rangle_{\text{bus}} \leftrightarrow |aux\rangle_k |0\rangle_{\text{bus}}$$

A $2\pi$ pulse (full Rabi cycle) on this transition sends:
- $|e\rangle_k |1\rangle_{\text{bus}} \to -|e\rangle_k |1\rangle_{\text{bus}}$ (picks up phase $-1$)
- $|g\rangle_k |1\rangle_{\text{bus}} \to |g\rangle_k |1\rangle_{\text{bus}}$ (off-resonant, unchanged)
- $|e\rangle_k |0\rangle_{\text{bus}} \to |e\rangle_k |0\rangle_{\text{bus}}$ (wrong phonon number, unchanged)
- $|g\rangle_k |0\rangle_{\text{bus}} \to |g\rangle_k |0\rangle_{\text{bus}}$ (doubly off-resonant)

This is a controlled-$Z$ gate between the bus mode and ion $k$'s qubit. The auxiliary level is transiently populated during the $2\pi$ rotation but returns to zero population at the end.

## When to reach for it
- Implementing conditional operations in AMO systems where direct qubit-qubit interaction is unavailable
- Any platform where a qubit has access to levels outside the computational subspace that can be selectively addressed
- Superconducting circuits also use this pattern (transmon anharmonicity provides "leakage levels" that enable conditional phases)

## Complexity
One laser pulse per controlled-phase gate. The pulse must be resonant with the $|e\rangle|1\rangle \leftrightarrow |aux\rangle|0\rangle$ transition and have precisely controlled area ($2\pi$). Gate fidelity depends on how well the auxiliary level is isolated from other transitions.

## Caveat
Requires a suitable auxiliary level with: (1) accessible transition from $|e\rangle$, (2) sufficiently different frequency from the qubit transition to avoid crosstalk, (3) long enough lifetime to survive the $2\pi$ rotation. Spontaneous decay from $|aux\rangle$ during the gate causes errors. Modern ion-trap gates (Mølmer-Sørensen, light-shift gates) avoid auxiliary levels entirely by using off-resonant sideband interactions to accumulate geometric phases.

## Related notes
- [[Quantum Computations with Cold Trapped Ions (Cirac-Zoller 1995) — Paper Notes]]
- [[Shared Bosonic Mode as Quantum Bus]]
- [[Gate Teleportation Protocol]]
