> **Source:** Berry, Gidney, Motta, McClean, Babbush, arXiv:1902.02134; building on Low, Kliuchnikov, Schaeffer, arXiv:1812.00954
> **Tags:** #trick #circuit-primitive #T-complexity #QROM #QROAM #data-loading #oracle #space-time-tradeoff

## What it does
Reduces the Toffoli cost of coherent table lookups from $O(d)$ (standard [[QROM (Quantum Read-Only Memory)|QROM]]) to $O(\sqrt{dM})$ by trading ancilla qubits for gate count, where $d$ is the number of table entries and $M$ is the output bit-width.

## The trick
Standard [[QROM (Quantum Read-Only Memory)|QROM]] sweeps through all $d$ addresses via [[Unary Iteration]], costing $O(d)$ Toffolis regardless of output size $M$. QROAM splits the address into high bits (selecting a "page" of $k$ entries) and low bits (selecting within the page):

**Compute with clean ancillae:**
1. Allocate $k$ output registers $r_0, \ldots, r_{k-1}$, each of $M$ qubits.
2. Use standard QROM on the top $\lceil\log(d/k)\rceil$ address bits to load all $k$ entries in the selected page simultaneously. Cost: $\lceil d/k \rceil$ Toffolis.
3. Use controlled swaps driven by the bottom $\log k$ address bits to route the correct entry into $r_0$. Cost: $M(k-1)$ Toffolis.

Total compute: $\lceil d/k \rceil + M(k-1)$. Optimal $k \approx \sqrt{d/M}$, giving $\sim 2\sqrt{dM}$ Toffolis.

**Compute with dirty ancillae (Theorem 1 of the paper):**
The system register and other unused qubits serve as "dirty" ancillae (not initialized to zero). The swapping network must be repeated twice (with Hadamards between) to cancel the dirty state's contribution:

Cost: $2\lceil d/k \rceil + 4M(k-1)$, using $(k-1)M$ dirty ancillae.

This is improved over Low, Kliuchnikov, Schaeffer (arXiv:1812.00954) by:
- Using a linear-depth swapping network instead of log-depth (saves factor of 2 in $Mk$ term, since dirty CSWAP must be toggled twice in the log-depth version)
- Using $|+\rangle$ states instead of spare registers (saves from $Mk$ to $M(k-1)$ ancillae)

**Uncomputation:** See [[Measurement-Based QROM Uncomputation]] for the $M$-independent uncomputation that complements QROAM.

## When to reach for it
- Any fault-tolerant algorithm bottlenecked by [[QROM (Quantum Read-Only Memory)|QROM]] lookups with large tables ($d \gg 1$)
- When ancilla qubits are available (clean or dirty) to trade for reduced Toffoli count
- State preparation for [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based chemistry, where $d = O(N^3)$ or more
- Any [[Coherent Alias Sampling for PREPARE|coherent alias sampling]] PREPARE oracle

## Complexity
| Setting | Compute cost | Uncompute cost | Ancilla |
|---|---|---|---|
| Clean, optimal $k$ | $\sim 2\sqrt{dM}$ | $\sim 2\sqrt{d}$ (with [[Measurement-Based QROM Uncomputation\|measurement uncompute]]) | $\sqrt{dM}$ clean |
| Dirty, $k$ bounded by $N$ | $2dM/N + O(N)$ | $\sim dN/4$ | $N$ dirty |

## Caveat
The space cost can be significant: optimal $k$ with clean ancillae requires $O(\sqrt{dM})$ extra qubits. For very large tables (e.g., $d = 10^6$), this can exceed the system register size. The dirty-ancilla variant is less costly in space but $\sim 4\times$ more expensive in Toffolis.

A lower bound proven in Low, Kliuchnikov, Schaeffer (arXiv:1812.00954) shows that no further space-time tradeoffs can asymptotically beat the $\sqrt{dM}$ scaling for general QROM.

## Related notes
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]]
- [[QROM (Quantum Read-Only Memory)]] — direct predecessor from [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]]. QROAM keeps the same fixed-table lookup model but adds a space-time tradeoff to beat QROM's linear-in-$d$ Toffoli cost.
- [[Coherent Alias Sampling for PREPARE]]
- [[Unary Iteration]]
- [[Measurement-Based QROM Uncomputation]]
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
