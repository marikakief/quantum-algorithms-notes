
> **Tags:** #trick #qrom #block-encoding #resource-tradeoff
> **Source:** arXiv:2510.08644 (Liu, Zhu, Lin, Low, Yang), Section V

## What it does

Loads $L$ coefficient values (for amplitude oracle $O_A$ in [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]]) with T-count $\tilde{O}(\sqrt{L})$ instead of the standard $O(L)$, by partitioning the table into groups and using a two-level lookup: [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] to identify the group, then SWAP to identify the entry within the group.

## The trick

Standard QROM/alias sampling loads all $L$ entries with one round of $O(L)$ Toffoli gates. The SELECT-SWAP approach:

1. **Partition:** Divide $L$ entries into $L/\lambda$ groups of size $\lambda$ each.
2. **[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] (outer level):** Use a quantum multiplexer to [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] the correct group register — costs $O(L/\lambda)$ T gates.
3. **SWAP (inner level):** Swap the selected entry from the $\lambda$-element group into a working register — costs $O(\lambda \cdot m_b)$ T gates ($m_b$ = bits per coefficient, typically $O(\log(L/\varepsilon))$).
4. **Optimize $\lambda$:** Set $\lambda = \sqrt{L/m_b}$, giving T-count $O(L/\lambda + \lambda m_b) = O(\sqrt{L m_b}) = \tilde{O}(\sqrt{L})$.

This is the amplitude oracle $O_A$. The sparsity oracle $O_C$ uses a different architecture — SWAP-only (without the [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] layer) — suited to the binary-tree structure of creation/annihilation operator connectivity.

## When to reach for it

- Large [[Linear Combination of Unitaries (LCU)|LCU]] coefficient tables in [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] PREPARE/[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] pipelines
- Any setting where the bottleneck is amplitude loading from a classical table with many entries
- When T-count dominates the circuit cost and Clifford+ancilla overhead is acceptable

## Complexity

T-count: $\tilde{O}(\sqrt{L \cdot \log(L/\varepsilon)})$ per oracle call. Compare to $O(L)$ for standard alias sampling. The improvement is most significant for large $L$ (e.g., $L \sim n^4$ for molecular Hamiltonians) and large-precision computations.

Ancilla overhead: $O(\lambda \cdot m_b) = O(\sqrt{L/\log(L/\varepsilon)} \cdot \log(L/\varepsilon))$ ancilla qubits — more than the standard approach but sublinear in $L$.

## Caveat

- The Clifford gate count and routing overhead increase. For resource estimation, this matters because Clifford gates contribute to circuit depth and thus decoherence time, even if they're "free" in T-count.
- The optimal $\lambda$ depends on $m_b$ (precision), which itself depends on the target approximation error $\varepsilon$ and the coefficient magnitudes. Needs careful tuning per problem instance.
- This addresses the [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] oracle cost; the [[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]] oracle (applying the $j$-th unitary) has separate cost structure.

## Related Paper Notes
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]]
- [[Comparator-Based Direct Amplitude Sampling from Fixed-Point Coefficients]]
