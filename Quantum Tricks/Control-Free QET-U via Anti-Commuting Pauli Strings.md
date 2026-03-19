# Control-Free QET-U via Anti-Commuting Pauli Strings

> **Source:** Yulong Dong, Lin Lin, and Yu Tong, arXiv:2204.05955 (PRX Quantum 2022), Section VI
> **Tags:** #trick #control-free #QET-U #spin-models #Trotter

## What it does

Eliminates the need for controlled Hamiltonian evolution in [[QET-U — Eigenvalue Transformation via Hamiltonian Evolution|QET-U]] for quantum spin Hamiltonians, replacing it with conjugation by **controlled Pauli strings**. The result: QET-U circuits using only controlled single-qubit gates (no controlled multi-qubit unitaries).

## The trick

If a Hamiltonian group $H^{(j)}$ anti-commutes with a Pauli string $K_j$ (i.e., $K_j H^{(j)} K_j = -H^{(j)}$), then:

$$K_j e^{-i\tau H^{(j)}} K_j = e^{i\tau H^{(j)}}$$

So conjugating the time evolution by $K_j$ **flips the sign** of the evolution time. To implement the controlled forward/backward evolution needed by QET-U, insert controlled-$K_j$ gates before and after each Trotter layer. Since $K_j$ is a Pauli string, the controlled version decomposes into controlled single-qubit Pauli gates — far cheaper than controlling the entire $e^{-iH}$.

For $H = \sum_{j=1}^\ell H^{(j)}$ with $\ell$ anti-commuting groups, the overhead is $O(d\ell)$ controlled Pauli strings for polynomial degree $d$. When all groups share the same $K$ ($\ell = 1$), controlled Pauli strings between Trotter layers cancel, and only $2d$ insertions are needed at the boundaries of each QET-U step.

### Worked examples

| Model | Groups $\ell$ | Anti-commuting $K$ |
|---|---|---|
| TFIM: $-\sum Z_jZ_{j+1} - g\sum X_j$ | 1 | $Y_1 Z_2 Y_3 Z_4 \cdots$ |
| Heisenberg: $-J_x\sum XX - J_y\sum YY - J_z\sum ZZ$ | 2 | $K_1 = Z_1 I_2 Z_3 I_4 \cdots$, $K_2 = X_1 I_2 X_3 I_4 \cdots$ |
| Fermi-Hubbard (Jordan-Wigner) | 3 | Three Pauli strings on the two-chain qubit layout |

## When to reach for it

- Implementing QET-U for spin model ground-state problems on near-term hardware where controlling $e^{-iH}$ directly is prohibitively expensive.
- Any Trotter-based simulation where anti-commuting structure exists and you want to avoid controlled two-qubit gates.
- The TFIM case is simplest ($\ell = 1$, maximal cancellation).

## Complexity

- **Additional gates per QET-U step:** $O(\ell n)$ controlled single-qubit Paulis (where $n$ is system size)
- For TFIM: $2d$ controlled Pauli strings total, each requiring $n$ controlled single-qubit gates
- Trotter step cost is unchanged; overhead is purely from the Pauli conjugation

## Caveat

- Only works when anti-commuting Pauli strings exist. For generic Hamiltonians (e.g., molecular Hamiltonians in second quantization with complex terms), this structure may not be available or $\ell$ may be large.
- When $\ell > 1$, the controlled Pauli strings don't cancel between Trotter layers, increasing overhead by factor $\ell r$ (where $r$ is Trotter steps).
- The circuit still uses **controlled** Pauli gates (controlled by the ancilla qubit). "Control-free" means no controlled multi-qubit unitaries, but single-qubit controlled gates remain.

## Related notes
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]]
- [[QET-U — Eigenvalue Transformation via Hamiltonian Evolution]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
