# QROM (Quantum Read-Only Memory)

> **Source:** Babbush, Gidney, Berry, Wiebe et al., arXiv:1805.03662, Phys. Rev. X **8**, 041015 (2018)
> **Tags:** #trick #circuit-primitive #T-complexity #QROM #data-loading #oracle

## What it does

Coherently loads classical lookup-table data into a quantum register: given an index superposition $\sum_\ell c_\ell |\ell\rangle|0\rangle$, produces $\sum_\ell c_\ell |\ell\rangle|d_\ell\rangle$ where $d_\ell$ is the precomputed classical data at address $\ell$. T-cost: $4L - 4$ for $L$ entries. Ancilla: $O(\log L)$ qubits.

## The trick

**Problem:** Implementing PREPARE (loading coefficient amplitudes) and other oracles requires coherently reading classical tables. Naive QRAM models often assume $O(1)$ T-gate cost per access, which is unrealistic in fault-tolerant architectures; QROM gives explicit and cheap Clifford+T circuits.

**Construction (Section III C, Fig. 10 of [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush 2018]]):**

1. Use [[Unary Iteration]] to sweep through index values $\ell = 0, 1, \ldots, L-1$.
2. For each index $\ell$, the classical data word $d_\ell$ is hard-coded into the circuit as CNOT gates (no T gates): for each bit $j$ of $d_\ell$, include a CNOT on output qubit $j$ controlled by the unary flag for position $\ell$ — if bit $j$ of $d_\ell$ is $1$, apply the CNOT; otherwise no gate.
3. After the full sweep, the output register holds $|d_\ell\rangle$ in superposition over the index.

Since the data encoding is via CNOTs (Clifford gates), the *only* T-gate cost is from the unary iteration itself: $4L - 4$.

**Uncomputation:** After the data is consumed by subsequent operations, the output register can be uncomputed (measure and classically correct), recovering the ancilla. Uncomputation costs 0 additional T gates.

**Key insight:** The classical data $\{d_\ell\}$ is baked into the circuit structure as presence/absence of CNOT gates, not dynamically looked up. This is why QROM is not the same as QRAM (no hardware bucket-brigade required).

**Precision and data size:** If each data word $d_\ell$ has $b$ bits, the output register is $b$ qubits, but the T-gate count remains $4L - 4$ regardless of $b$. Only the number of CNOTs grows linearly in $b$.

## When to reach for it

- Loading precomputed coefficient tables in PREPARE oracles (LCU weights, alias sampling tables).
- Implementing any oracle where classical function values need to be loaded coherently.
- Fault-tolerant settings where T gates are the expensive resource (surface code).
- Any situation where [[Unary Iteration]] applies and you need data-dependent operations.

## Complexity

- T gates: $4L - 4$ (independent of word size $b$)
- Additional Clifford (CNOT) gates: $O(Lb)$
- Ancilla qubits: $O(\log L + b)$
- Uncomputing the output register: 0 additional T gates

## Caveat

- Classical circuit: the circuit must be recompiled if the classical data changes. QROM is for *read-only* data. Unlike QRAM, you cannot update entries at runtime.
- $O(L)$ T-gate cost: for $L = O(N^2)$ oracle entries, this gives $O(N^2)$ T gates just for data loading — acceptable here because SELECT also costs $O(N)$, but it's not free.
- No quantum addressing: QROM does not implement a general "quantum addressable" memory. It is a compiled, fixed-table lookup. Hardware QRAM (bucket brigade) is a different device with different trade-offs.
- Depth is $O(L)$: the sequential sweep is not depth-efficient. When circuit depth is the bottleneck, consider parallelized QROM variants (at the cost of more ancilla).

## Related notes

- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — introduced here
- [[QROAM (Space-Time Tradeoff for QROM)]] — later refinement from [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry et al. 2019]] that trades extra ancilla for lower Toffoli count. QROM is the earlier baseline; QROAM is the scaled-up version for larger tables.
- [[Unary Iteration]] — QROM is built on top of unary iteration
- [[Coherent Alias Sampling for PREPARE]] — uses QROM to load alias tables
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — QROM is used inside the PREPARE oracle for qubitization
