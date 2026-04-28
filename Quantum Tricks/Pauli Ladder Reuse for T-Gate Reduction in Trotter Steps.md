# Pauli Ladder Reuse for T-Gate Reduction in Trotter Steps

## What it does

In a Jordan-Wigner second-quantized Hamiltonian, the Pauli terms share long strings of $Z$ operators. When implementing the product formula step $\prod_j e^{-i\theta_j P_j}$ in a circuit, adjacent Pauli exponentials that differ only in their final $X$ or $Y$ gate can share the entire CNOT ladder. This trick identifies and exploits such shared structure to reduce CNOT and T-gate count.

## The trick

A generic Pauli exponential $e^{-i\theta Z_1 Z_2 \cdots Z_k X_{k+1}}$ compiles to: a ladder of $k$ CNOTs collecting parity into an ancilla, a single $R_z(2\theta)$ rotation on the ancilla, and the reverse CNOT ladder. The $R_z$ is the only non-Clifford gate (costing $O(\log(1/\epsilon))$ T gates via synthesis).

When two consecutive terms $P_j$ and $P_{j+1}$ in the Trotter product share the same $Z$-string up to the last site, the CNOT ladders for both can be collapsed: run the forward ladder once, apply $R_z(\theta_j)$, change the final Pauli, apply $R_z(\theta_{j+1})$, then run the reverse ladder once. This saves one full CNOT ladder pair (roughly $O(N)$ CNOTs for an $N$-qubit Pauli string).

More generally, sort the Pauli terms into groups sharing maximal common prefixes and implement them in a single shared-ladder block. For a Jordan-Wigner Hamiltonian with $O(N^4)$ terms but only $O(N^2)$ distinct $Z$-string prefixes, the savings can be roughly quadratic.

The trick was exploited in [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes|Childs-Maslov-Nam-Ross-Su 2018]] to reduce the effective circuit depth per Trotter step and is the standard optimization in quantum chemistry Trotter compilers.

## When to reach for it

- Compiling second-quantized (Jordan-Wigner) Hamiltonian product-formula steps.
- Any Hamiltonian with structured Pauli terms that share bitstring prefixes.
- When T-gate count dominates and CNOT counts are cheap relative to T gates.

## Complexity

Naive per-term circuit: $O(N^4)$ Pauli terms $\times$ $O(N)$ CNOTs + 1 rotation each $= O(N^5)$ gates per step.

With ladder reuse, shared-prefix grouping reduces CNOT count by a factor of $O(N)$ on average for Jordan-Wigner terms, giving $O(N^4)$ CNOTs and $O(N^4)$ rotations per step. T-gate count per step is $O(N^4 \log(1/\epsilon_S))$.

## Caveat

- The ordering of Pauli terms within the Trotter product must respect commutativity constraints; aggressively reordering to maximize prefix sharing can increase Trotter error if it groups non-commuting terms.
- The savings depend strongly on the Hamiltonian's Pauli structure; for Hamiltonians already in a geometrically local form (e.g., lattice models), prefix sharing is less useful.
- This trick reduces CNOT counts significantly but the synthesis cost (T gates per rotation) remains; for very high precision $\epsilon$, the synthesis cost dominates over CNOT savings.

## Related notes

- [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes]] — source paper
- [[Error Budget Splitting for Trotter-Plus-Synthesis Circuits]] — companion optimization
- [[Trotterized Time Evolution for Chemistry]] — broader context
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — related Trotter error analysis in chemistry
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — alternative approach that avoids per-rotation synthesis entirely
- [[Edge-Coloring Decomposition for Sparse Hamiltonians]] — related decomposition trick
