# SelectSwap Network for Data Lookup

> **Source:** Low, Kliuchnikov, Schaeffer, arXiv:1812.00954
> **Tags:** #trick #circuit-primitive #T-complexity #data-loading #QROM #dirty-qubits

## What it does

Implements a classical data-lookup oracle $O|x\rangle|0\rangle = |x\rangle|a_x\rangle$ for $N$ entries of $b$ bits with T-count $O(\sqrt{bN})$, using dirty qubits to achieve a quadratic improvement over standard [[QROM (Quantum Read-Only Memory)|QROM]].

## The trick

Split the index $x = q\lambda + r$ into quotient and remainder. The oracle decomposes into two parts:

1. **Select** (controlled by $|q\rangle$): Use a multiplexer (via [[Unary Iteration]]) to write $\lambda$ consecutive data entries into $\lambda$ parallel $b$-bit registers. T-cost: $O(N/\lambda)$.
2. **Swap** (controlled by $|r\rangle$): Route the desired entry from register $r$ to the output. T-cost: $O(b\lambda)$.

Total: $O(N/\lambda + b\lambda)$, minimised at $\lambda = O(\sqrt{N/b})$ giving $O(\sqrt{bN})$.

The $b\lambda$ qubits holding the parallel registers can be **dirty** — in any unknown computational basis state. The circuit uses an XOR-swap-restore pattern:

$$|0\rangle|\phi\rangle \xrightarrow{\text{Select}} |0\rangle|\phi \oplus a_x\rangle \xrightarrow{\text{Swap}} |a_x \oplus \phi_r\rangle|\phi \oplus a_x\rangle \xrightarrow{\text{Select}^\dagger} |a_x \oplus \phi_r\rangle|\phi\rangle \xrightarrow{\text{Swap}} |a_x\rangle|\phi\rangle$$

By linearity, this works for arbitrary initial states of the dirty qubits — including entangled states.

## When to reach for it

- Any algorithm that needs coherent access to a classical lookup table and has idle qubits available (which is most fault-tolerant algorithms).
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Qubitization]] and [[Linear Combination of Unitaries (LCU)|LCU]] algorithms where the PREPARE oracle loads Hamiltonian coefficients.
- [[Coherent Alias Sampling for PREPARE|Alias sampling]] circuits that need to load alias tables.
- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes|Quantum data regression]] and other data-loading applications.
- Any time [[QROM (Quantum Read-Only Memory)|QROM]] is the bottleneck and you have $\Omega(\sqrt{N})$ idle qubits.

## Complexity

| Parameter | Cost |
|-----------|------|
| T-count | $O(N/\lambda + b\lambda)$, optimal $O(\sqrt{bN})$ |
| Clean qubits | $b + 2\lceil\log_2 N\rceil$ |
| Dirty qubits | $b\lambda$ |
| T-depth | $O(N/\lambda + \log\lambda)$ |

## Caveat

- Requires $O(\sqrt{N/b})$ dirty qubits for the full quadratic speedup. With fewer dirty qubits the improvement is proportional to $\lambda$.
- The Clifford count is *not* reduced — only the T-count benefits.
- The optimality proof (matching lower bound $q\Gamma = \Omega(bN - q^2)$) holds only when $\lambda = o(\sqrt{N/b})$, i.e., when T gates dominate the qubit count.
- [[QROAM (Space-Time Tradeoff for QROM)|QROAM]] (Berry, Gidney et al. 2019) refines this by adding [[Measurement-Based QROM Uncomputation|measurement-based uncomputation]], reducing the cost of the reverse pass. For most practical chemistry applications, QROAM is the preferred implementation.

## Related notes
- [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes]]
- [[QROM (Quantum Read-Only Memory)]] — the $\lambda = 1$ special case
- [[QROAM (Space-Time Tradeoff for QROM)]] — refined version with measurement-based uncomputation
- [[Unary Iteration]] — used inside the Select component
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]]
- [[Dirty Qubit Recycling for T-Gate Reduction]]
