# Rectangular Multiport Unitary Synthesis

> **Source:** Berry, Tong, Khattar, White, Kim, Boixo, Lin, Lee, Chan, Babbush, Rubin, arXiv:2409.11748 (2024)
> **Tags:** #trick #unitary-synthesis #fault-tolerant #QROM #Toffoli #circuit-compilation

## What it does

Synthesises an arbitrary $N_{\text{un}} \times N_{\text{un}}$ unitary operation using alternating layers of [[QROM (Quantum Read-Only Memory)|QROM]]-loaded phase rotations and Hadamard gates, with Toffoli cost $\sim 7\times$ lower than the LKS (Low-Kliuchnikov-Schaeffer) method.

## The trick

Reinterpret the Clements et al. (2016) rectangular multiport interferometer decomposition as a quantum circuit. An $N_{\text{un}}$-mode interferometer decomposes into $N_{\text{un}}$ alternating layers of $N_{\text{un}}/2$ beam splitters. Each beam splitter is two 50/50 beam splitters (Hadamards) with phase shifts in between.

The phases from successive layers can be absorbed into each other, leaving:
- $N_{\text{un}}$ layers of two phase rotations each, controlled by the first $n - 1$ qubits
- A Hadamard on the last qubit between each layer
- One final layer of $N_{\text{un}}$ phases

Each pair of phase rotations is loaded by a single [[QROM (Quantum Read-Only Memory)|QROM]] lookup on the first $n - 1$ qubits. The QROM is erased after the rotations, and the sign fixup from erasure is folded into subsequent phase layers at zero additional Toffoli cost.

**QROM cost per layer:**

$$\left\lfloor \frac{N_{\text{un}}}{2\Lambda} \right\rfloor + (2\Lambda b - 5)$$

where $\Lambda$ is the QROM branching factor and $b$ is the rotation precision in bits.

**Total cost includes:**
- $N_{\text{un}}$ QROM calls for the phase/Hadamard layers
- $(n - 2)(N_{\text{un}} - 1)$ Toffolis for increment/decrement operations (handling even vs. odd layer block alignment)
- One final QROM for the terminal phases

The increment/decrement cost $(n - 2)$ per operation comes from modular addition on $n - 1$ qubits.

## When to reach for it

- MPS state preparation, where each site requires synthesising a unitary (or partial columns of a unitary) of dimension $d\chi$.
- Any fault-tolerant algorithm that needs to implement a general unitary specified by classical data, where the Toffoli cost of the unitary dominates.
- Situations where [[QROM (Quantum Read-Only Memory)|QROM]] is cheap relative to other operations — the method trades $N_{\text{un}}$ QROM lookups for a $7\times$ reduction in total Toffoli count vs. LKS.
- For unitaries up to $\sim 8$ qubits, an alternative approach using interspersed Hadamards and diagonal phasing (without the multiport decomposition) can be even more efficient, but the phase-finding problem becomes intractable for larger dimensions.

## Complexity

Total Toffoli cost for an $N_{\text{un}}$-dimensional unitary: $O(N_{\text{un}} \sqrt{N_{\text{un}}})$ (with optimised QROM branching), which is $\sim 7\times$ lower than LKS across a wide parameter range.

For MPS preparation with bond dimension $\chi$, physical dimension $d$, and $N$ sites: the dominant cost is $\sim (d - 1) \times N$ applications of the column synthesis variant (see [[Column Synthesis via SVD-QR Decomposition]]).

## Caveat

- LKS has better scaling with the physical dimension $d$: LKS scales as $\sqrt{d/2}$ while this method scales linearly in $d$. The crossover where LKS becomes more efficient is $d \approx 30$. For quantum chemistry ($d = 4$), this method wins by $\sim 3.5\times$.
- The method requires classical precomputation of the beam splitter parameters (Givens angles), which is $O(N_{\text{un}}^3)$ — not a bottleneck for practical sizes.

## Related notes
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]]
- [[Column Synthesis via SVD-QR Decomposition]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Unary Iteration]]
- [[Phase Gradient State for Controlled Rotations]]
