
> **Source:** Raeisi, Wiebe & Sanders, arXiv:1108.4318 (2012)
> **Tags:** #trick #circuit-synthesis #hamiltonian-simulation #Pauli-string #product-formula

## What it does

Synthesises the unitary $e^{-i\theta P}$ for any $k$-body Pauli string $P = \bigotimes_{q \in S} \sigma_q$ using $O(k)$ CNOTs, $O(k)$ single-qubit gates, and one $R_z$ rotation.

## The trick

1. **Diagonalise each Pauli factor.** For every qubit $q$ in the support:
   - If $\sigma_q = X$: apply $H$ (maps $X \leftrightarrow Z$).
   - If $\sigma_q = Y$: apply $H S^\dagger$ (or equivalently $H$ then a $\sqrt{Z}^\dagger$ rotation, maps $Y \to Z$).
   - If $\sigma_q = Z$: no change.

2. **Compute parity into target qubit.** Pick a target $\ell$ (convention: highest-index qubit in support). Apply CNOTs from each other support qubit into $\ell$:

$$\ell \;\leftarrow\; \ell \oplus q_1 \oplus q_2 \oplus \cdots \oplus q_{k-1}.$$

After this, $\ell$ holds the overall $Z$-parity of the string $P$ in the computational basis.

3. **Apply $R_z$.** On qubit $\ell$ apply $R_z(2\theta)$. This implements $e^{-i\theta Z_\ell}$ which, conditional on the parity, is exactly $e^{-i\theta P}$ on the original qubits.

4. **Uncompute.** Reverse the CNOT ladder and the single-qubit basis changes (steps 2→1 in reverse).

No ancilla qubits are needed; the target qubit is part of the system register.

**Circuit shape** (example $P = X_1 Y_2 Z_3$, target = qubit 3):

```
q1: ─H──●──────────H──
        │
q2: ─HS†┼──●──────S H─
         │  │
q3: ─────X──X──Rz──X──X─
```

## When to reach for it

- Implementing any product-formula (Trotter/Suzuki) step for a Pauli-string Hamiltonian $H = \sum_j a_j h_j$.
- Compiling VQE ansatz layers (each variational rotation is exactly this pattern).
- Whenever you need $e^{-i\theta P}$ for arbitrary multi-qubit Pauli $P$ from a finite Clifford+T or Clifford+$R_z$ gate set.

## Complexity

- CNOTs: $2(k-1)$ (CNOT ladder in + CNOT ladder out).
- Single-qubit gates: $2k$ (basis changes in + basis changes out).
- Rotations: 1 $R_z(2\theta)$ (approximated via [[Solovay-Kitaev Recursive Gate Compilation]] if discrete gate set needed: $O(\log^{3.97}(1/\delta))$ additional gates for error $\delta$).
- Total per primitive: $O(k + \mathrm{polylog}(1/\delta))$.

## Caveat

- Requires knowing the support $S$ of $P$ explicitly (not applicable to black-box Hamiltonians).
- The CNOT count is $2(k-1)$, not $O(1)$; for dense $k$-body Hamiltonians (large $k$) or many terms, this accumulates.
- CNOT ladders from different Pauli exponentials can only be parallelised if the primitives act on disjoint qubit sets — use [[Commuting-Group Parallelisation via Graph Colouring]] to schedule them concurrently.

## Related notes
- [[Quantum-Circuit Design for Efficient Simulations of Many-Body Quantum Dynamics (Raeisi-Wiebe-Sanders 2012) — Paper Notes]]
- [[Commuting-Group Parallelisation via Graph Colouring]]
- [[Solovay-Kitaev Recursive Gate Compilation]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
- [[Suzuki Recursion for Ordered Exponentials]]
