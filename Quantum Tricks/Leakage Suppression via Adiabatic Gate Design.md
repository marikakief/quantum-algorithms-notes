# Leakage Suppression via Adiabatic Gate Design

> **Source:** Jordan, Krovi, Lee, Preskill, arXiv:1703.00454
> **Tags:** #trick #adiabatic #gate-design #leakage #quantum-field-theory

## What it does
Implements quantum gates on a computational subspace embedded in a larger Hilbert space, using smooth parameter variation to suppress leakage via the adiabatic theorem.

## The trick
When two qubits interact (e.g., two particles in potential wells), the full low-energy Hilbert space $S$ may be larger than the computational subspace $C$. In the JLP construction, two particles in four wells span a $\binom{4}{2} = 6$-dimensional subspace $S$, while the computational subspace $C$ is 4-dimensional (one particle per double-well).

By smoothly varying the potential landscape (well depths and separations) as a function of time, the system evolves unitarily within $S$. If the time variation belongs to the Gevrey class of order $\alpha$, the Nenciu-Rasche adiabatic theorem gives **exponential** suppression of transitions out of $S$:

$$\varepsilon_{\text{leak}} \sim \exp\!\left(-\tau^{1/(1+\alpha)}\right)$$

where $\tau$ is the gate time. This is much stronger than the polynomial $O(1/\tau)$ suppression from the standard adiabatic theorem.

The key insight: even though the unitary on $S$ isn't directly a gate on $C$, it can be chosen (by appropriate well trajectories) to act as an entangling gate when restricted to $C$. The extra dimensions of $S$ are "workspace" that the adiabatic evolution uses but ultimately returns the state to $C$.

## When to reach for it
- Any setting where the computational subspace sits inside a larger physical Hilbert space and direct gate operations risk leakage (superconducting qubits with higher transmon levels, atoms with additional Zeeman sublevels, molecular vibrations)
- BQP-hardness proofs where error correction is unavailable and each gate must achieve $O(1/G)$ infidelity directly
- Designing smooth pulse shapes for quantum gates where leakage is the dominant error source

## Complexity
Gate time $\tau = O(\lambda^{-2})$ per two-qubit gate. For a circuit with $G$ gates and $\lambda = O(1/G)$: $\tau = O(G^2)$ per gate, $O(G^2 D)$ total for depth $D$.

## Caveat
Requires a spectral gap separating the computational subspace from higher-energy states throughout the entire gate operation. If the gap closes at any point during the parameter sweep, the adiabatic protection fails. Also, the Gevrey-class smoothness requirement is stronger than $C^\infty$ — it constrains the growth rate of derivatives, not just their existence. In practice, this means the parameter variation can't have sharp features.

## Related notes
- [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes]]
- [[Qubit Encoding in Double-Well Potentials]]
- [[Adiabatic Passage for Particle Creation from Vacuum]]
- [[Adiabatic Approximation Boundary-Term Dominance]]
