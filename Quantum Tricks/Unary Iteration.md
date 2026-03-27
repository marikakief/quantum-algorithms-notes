
> **Source:** Babbush, Gidney, Berry, Wiebe et al., arXiv:1805.03662, Phys. Rev. X **8**, 041015 (2018)
> **Tags:** #trick #circuit-primitive #T-complexity #SELECT #indexed-operations #unary

## What it does

Implements an indexed controlled operation $\sum_{\ell=0}^{L-1} |\ell\rangle\langle\ell| \otimes U_\ell$ over $L$ possible unitaries $U_\ell$, given an index register $|\ell\rangle$ stored in $\lceil\log L\rceil$ binary bits, using T-gate count $4L - 4$ and $O(\log L)$ ancilla qubits.

## The trick

**Problem:** You have a binary index register $|\ell\rangle$ and want to apply $U_\ell$ to a target system conditioned on $|\ell\rangle = \ell$ for all $L$ values. A naive approach (one multi-controlled gate per value) costs $O(L \log L)$ T gates.

**Solution:** Convert the binary index to a *unary* (one-hot) representation on-the-fly while sweeping through the $L$ possible values, applying each $U_\ell$ controlled on the single unary bit for $\ell$, then uncomputing the unary expansion.

**Circuit construction (from the paper, Section III A, Figs. 3–7):**

1. Start with the binary register $|\ell\rangle = |b_{k-1}, \ldots, b_0\rangle$ (where $k = \lceil\log L\rceil$).
2. Build a binary tree of AND gates that compute, for each $i \in \{0, \ldots, L-1\}$, the conjunction of bits asserting $|\ell\rangle = i$. Each AND (Toffoli) costs 4 T gates (using the phase-kickback AND construction with ancilla).
3. The sweep passes through the $L$ targets in order, maintaining a *unary flag* register: bit $i$ is $1$ if the sweep has reached (or passed) position $i$.
4. At each position $i$, apply $U_i$ controlled on the single unary flag bit.
5. **Optimization (Fig. 6):** adjacent AND computations share sub-expressions; after unary bit $i$ is no longer needed (all controlled $U_j$ for $j \leq i$ have been applied), uncompute the AND gate for free by re-running the computation backward. This eliminates many T gates.
6. Final count: each AND gate costs 4 T gates at compute time; uncomputing requires 0 additional T gates (measure and classically correct). Total: $4(L-1) = 4L - 4$ T gates for $L$ branches.

**Space:** Only $O(\log L)$ ancilla qubits are needed at any time — the unary expansion is not materialized all at once; bits are computed and immediately consumed.

**XOR accumulator trick (for Jordan–Wigner strings):** While sweeping, XOR each new unary bit into an accumulator register. The accumulator then holds the parity of "how many sweep steps have been applied so far," enabling efficient implementation of Jordan–Wigner $Z$-string operators (see [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|the chemistry paper]], Section III B).

## When to reach for it

- Building a SELECT oracle: $\sum_\ell |\ell\rangle\langle\ell| \otimes H_\ell$ for LCU decompositions.
- Implementing [[QROM (Quantum Read-Only Memory)]] (QROM uses unary iteration with data-encoding CNOTs).
- Any circuit where you need to apply different operations controlled on different values of an index register, and T-gate count matters.
- Jordan–Wigner string operators (hopping terms in chemistry/Hubbard Hamiltonians).

## Complexity

- T gates: $4L - 4$
- Ancilla qubits: $O(\log L)$
- Circuit depth: $O(L)$ in general; can be reduced with parallelism at cost of more ancilla

## Caveat

- Linear in $L$ — fine when $L = O(N)$ (this paper's setting), but if $L = O(N^4)$ (naive second-quantized LCU) this becomes expensive. The whole point of the plane-wave dual basis is to keep $L = O(N^2)$.
- Sequential structure makes it depth-inefficient. When circuit depth (not T count) is the bottleneck, different approaches may be better.
- The AND gate uncomputation trick (Fig. 4 of the paper) relies on measuring the ancilla and classically conditioning future gates — not standard in all fault-tolerance frameworks.

## Related notes

- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — introduced here; used throughout SELECT and QROM
- [[Unary Encoding for Sequential Controlled Operations]] — earlier unary-control pattern from [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry et al. 2015]] for variable-length products. That card is about unary-encoding the Taylor order $k$; this card is the later indexed-sweep primitive for binary addresses and is the more direct ancestor of QROM.
- [[QROM (Quantum Read-Only Memory)]] — built on top of unary iteration
- [[Coherent Alias Sampling for PREPARE]] — uses QROM which uses unary iteration
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the SELECT oracle in qubitization uses unary iteration
- [[Controlled Swap Network for Select Oracle]] — alternative approach used in real-space simulation
