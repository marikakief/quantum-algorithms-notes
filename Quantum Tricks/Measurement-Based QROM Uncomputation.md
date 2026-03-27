
> **Source:** Berry, Gidney, Motta, McClean, Babbush, arXiv:1902.02134 (Appendix C)
> **Tags:** #trick #circuit-primitive #T-complexity #QROM #uncomputation #measurement #phase-fixup

## What it does
Uncomputes a [[QROM (Quantum Read-Only Memory)|QROM]] table lookup (erasing the output register) at cost $O(\sqrt{d})$ Toffolis, **independent of the output bit-width $M$**. Standard reversal-based uncomputation would cost as much as the forward computation — this trick removes the $M$-dependence entirely.

## The trick
A QROM lookup maps $\sum_j \psi_j |j\rangle|0\rangle \to \sum_j \psi_j |j\rangle|f(j)\rangle$. To uncompute $|f(j)\rangle$ back to $|0\rangle$:

1. **Measure all $M$ output qubits in the X basis.** This collapses each output qubit but does not disturb the superposition over $j$ (the output was a deterministic function of $j$, so the measurement results are classical side-information).

2. **Compute a classically conditioned phase fixup.** From the measurement outcomes, determine which address states $|j\rangle$ need a phase correction of $-1$. Let $S$ be the set of addresses needing negation. The fixup reduces to a new, much smaller table lookup:
   - Split the address register: let $q$ be the least significant bit and $h$ the remaining bits.
   - Define a "fixup table" $F$ with $d/2$ entries and 2-bit output, encoding which combinations of $(h, q)$ are in $S$.
   - Apply this fixup table using [[QROAM (Space-Time Tradeoff for QROM)|QROAM]].

3. **Result:** The output register is now $|0\rangle$ (from the X-measurement collapse), and the phases are correct (from the fixup).

The fixup table has size $d/2$ and output size 2 bits, so with $k$ clean ancillae the cost is $\lceil d/k \rceil + k$. At optimal $k \approx \sqrt{d}$: cost $\sim 2\sqrt{d}$.

With dirty ancillae: $2\lceil d/k \rceil + 4k$, optimal $k \approx \sqrt{d/2}$, cost $\sim \sqrt{32d}$.

The principle is the deferred measurement optimization: any circuit that uncomputes a qubit via a sequence of X-controlled operations can be replaced by an X-basis measurement followed by classically conditioned Z corrections (which are free in the surface code).

## When to reach for it
- Whenever a QROM output register must be erased after its contents have been consumed by downstream operations
- Particularly high-value when $M$ is large (many output bits) — the savings over reversing the forward computation scale linearly in $M$
- Standard companion to [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] in any fault-tolerant algorithm

## Complexity
- **Clean ancillae:** $\lceil d/k \rceil + k$ Toffolis; $k + \lceil\log(d/k)\rceil$ clean qubits; optimal $\sim 2\sqrt{d}$ Toffolis
- **Dirty ancillae:** $2\lceil d/k \rceil + 4k$ Toffolis; $k-1$ dirty + $\lceil\log(d/k)\rceil + 1$ clean; optimal $\sim \sqrt{32d}$

## Caveat
Requires mid-circuit measurement and classical feedforward. In the surface code this is natural (measurements are the basic operation), but in other architectures it may introduce latency. The classically conditioned phase corrections are Pauli Z gates, which are free in the surface code but not necessarily in other error-correcting codes.

The fixup table $F$ must be computed classically in real time from the measurement outcomes. For large $d$, this classical computation (essentially a bitwise inner product) must complete within the coherence window.

## Related notes
- [[QROAM (Space-Time Tradeoff for QROM)]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
