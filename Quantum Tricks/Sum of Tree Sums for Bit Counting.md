
> **Source:** Technique from Kivlichan, Gidney, Berry et al. (2020); compiled with improvements in Sanders, Berry et al., arXiv:2007.07391
> **Tags:** #trick #arithmetic #reversible-computation #ancilla-reduction

## What it does
Sums $L$ bits into a $\lceil \log(L+1) \rceil$-bit register using fewer than $2L$ Toffoli gates and only $O(\log L)$ ancilla qubits.

## The trick
A tree sum of $L$ bits costs $L - 1$ Toffolis but requires $L - 1$ ancilla (the tree structure). To reduce ancilla:

1. Divide the $L$ bits into $M \approx L / \lceil \log L \rceil$ groups of size $\lceil \log L \rceil$ each.
2. For each group, compute the tree sum (cost $\lceil \log L \rceil - 1$ Toffolis, using $\lceil \log L \rceil - 1$ temporary ancilla).
3. Add the group sum into a running total register (cost $\leq \lceil \log(L+1) \rceil - 1$ Toffolis).
4. Uncompute the group sum (free — Clifford gates plus measurement, no Toffolis).

Total Toffoli cost: $L - M - 1$ (tree sums) $+ M \lceil \log L \rceil$ (additions) $< 2L$.

The tree sums use $\lceil \log L \rceil - 1$ temporary ancilla (reused across groups). The running total uses $\lceil \log(L+1) \rceil$ persistent ancilla. Total ancilla: $O(\log L)$.

Compare with naive approaches:
- Direct tree sum: $L - 1$ Toffolis, $L - 1$ ancilla (too much space)
- Sequential addition: $L \log L$ Toffolis, $\log L$ ancilla (too many gates)
- This trick: $< 2L$ Toffolis, $O(\log L)$ ancilla (best of both worlds)

## When to reach for it
- Computing Hamming weights, population counts, or sums of binary-valued quantities in reversible circuits.
- Cost function evaluation for problems where the Hamiltonian is a sum of $\pm 1$ terms (like the SK model).
- Any reversible computation where you need to sum many single-bit contributions and ancilla is limited.

## Complexity
- Toffoli cost: $< 2L$
- Persistent ancilla: $\lceil \log(L+1) \rceil$ (output register)
- Temporary ancilla: $2\log L + O(1)$

When paired with uncomputation (compute + uncompute the sum), the average Toffoli cost drops to $< L$ because the tree sums can be uncomputed with no Toffoli cost using Gidney's measurement-based uncomputation.

## Caveat
The $< 2L$ bound is asymptotic; for small $L$ the group size $\lceil \log L \rceil$ doesn't buy much. The technique is most useful when $L$ is large enough that $L / \log L \ll L$. For $L < 16$ or so, a direct tree sum is simpler.

## Related notes
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]]
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]]
- [[Unary Iteration]]
