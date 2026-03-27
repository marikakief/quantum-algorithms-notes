# Dirty Qubit Recycling for T-Gate Reduction

> **Source:** Low, Kliuchnikov, Schaeffer, arXiv:1812.00954
> **Tags:** #trick #circuit-primitive #T-complexity #dirty-qubits #fault-tolerant #resource-estimation

## What it does

Converts idle qubits in a larger algorithm — qubits encoding coherent information but not currently needed — into a T-gate reduction for a subroutine, without initialising them or disturbing their state.

## The trick

A **dirty qubit** starts in some unknown computational-basis state $|\phi\rangle$ and must be returned to exactly that state. You can still use it to save T gates, via the XOR-swap-restore pattern:

1. **XOR on:** Apply your operation, XORing data onto the dirty register: $|0\rangle|\phi\rangle \to |0\rangle|\phi \oplus d\rangle$
2. **Copy out:** Swap/copy the result $d$ to a clean output register
3. **XOR off:** Reverse the operation to restore $|\phi\rangle$

This works by linearity even when $|\phi\rangle$ is entangled with other parts of the computation. The net effect: the dirty register is unchanged, but you've parallelised the computation across $\lambda$ dirty registers instead of processing $N$ items sequentially.

The key insight is that dirty qubits are **free** in many fault-tolerant algorithms. In [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]] or [[Linear Combination of Unitaries (LCU)|LCU]], the system register ($n$ qubits encoding the state being simulated) is idle during the PREPARE step. These qubits can serve as dirty ancillae for the [[SelectSwap Network for Data Lookup|SelectSwap network]], reducing T-count from $O(N)$ to $O(\sqrt{N})$.

## When to reach for it

- Fault-tolerant resource estimation: when counting T gates, check whether idle qubits could be borrowed as dirty ancillae.
- Any subroutine where parallelism reduces non-Clifford cost (data lookup, controlled-swap networks, multi-target CNOT).
- The [[SelectSwap Network for Data Lookup]] is the primary application, but the pattern generalises.
- Addition circuits also exhibit T-count/dirty-qubit tradeoffs (Gidney 2018), and so do AND gates (Barenco et al. 1995).

## Complexity

The benefit is proportional to the number of dirty qubits $\lambda$ available:
- T-count reduction: factor of $\lambda$ (up to the optimal $\lambda = O(\sqrt{N/b})$)
- No additional clean qubits required beyond $O(\log N)$
- No initialisation cost (no need for $|0\rangle$ preparation or magic-state distillation)

## Caveat

- Dirty qubits cannot be used for magic-state distillation — they don't replace T-factories, they reduce the number of T gates *needed*.
- The qubits must be returned to their original state. Any residual entanglement between the dirty qubits and the output corrupts the computation.
- The XOR-swap-restore pattern doubles the circuit depth compared to a clean-qubit version (you apply the operation twice — once forward, once backward).
- In architectures where all qubits are busy (no idle qubits), this trick offers no advantage.

## Related notes
- [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes]]
- [[SelectSwap Network for Data Lookup]] — primary application
- [[QROM (Quantum Read-Only Memory)]]
- [[QROAM (Space-Time Tradeoff for QROM)]]
- [[Error Budget Optimization for Fault-Tolerant Algorithms]]
