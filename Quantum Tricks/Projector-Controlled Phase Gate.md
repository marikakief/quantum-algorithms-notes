# Projector-Controlled Phase Gate

> **Source:** Martyn, Rossi, Tan & Chuang, *Grand Unification of Quantum Algorithms*, PRX Quantum **2**, 040203 (2021), Fig. 3. Also implicit in [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. 2019]].
> **Tags:** #trick #QSVT #QET #circuit #block-encoding

## What it does

Implements $e^{i\phi\Pi}$ where $\Pi = |\tilde{0}\rangle\langle\tilde{0}| \otimes I$ is the projector onto the "ancilla zero" subspace of a block-encoding. This is the multi-qubit replacement for the single-qubit QSP phase gate $S(\phi)$.

This gate is what makes the [[QSP-to-QSVT Lifting Chain]] work: every QSP phase $e^{i\phi\sigma_z}$ lifts to a projector-controlled phase $e^{i\phi\Pi}$ in the block-encoded setting.

## Construction

Let the ancilla register have $m$ qubits, and $|\tilde{0}\rangle = |0\rangle^{\otimes m}$.

**Circuit for $e^{i\phi\Pi}$:**
1. Apply $(m-1)$ controlled-NOT gates to reduce the condition "$\text{ancilla} = |0\rangle^{\otimes m}$" to a single qubit
2. Apply a $Z$ rotation $R_z(2\phi) = \text{diag}(e^{i\phi}, e^{-i\phi})$ to that control qubit
3. Uncompute the controlled-NOT gates

More precisely: flip an ancilla qubit $|q\rangle$ if and only if the remaining $m-1$ ancilla qubits are all zero (using a multi-controlled Toffoli or chain of CNOTs). Then:
$$e^{i\phi\Pi} = \text{(uncompute)} \cdot R_z^{(q)}(2\phi) \cdot \text{(flip if }\tilde{0}\text{)}$$

This costs $O(m)$ gates beyond the $Z$ rotation.

**Action:**
$$e^{i\phi\Pi} |\tilde{0}\rangle|\psi\rangle = e^{i\phi}|\tilde{0}\rangle|\psi\rangle$$
$$e^{i\phi\Pi} |\tilde{0}^\perp\rangle|\psi\rangle = |\tilde{0}^\perp\rangle|\psi\rangle$$

So it applies phase $e^{i\phi}$ inside the block-encoded subspace and does nothing outside it.

## Why it matters

This gate is the **bridge** from QSP to QET/QSVT. In QSP:
$$U_{\text{QSP}} = S(\phi_0)\prod_j [W(a) S(\phi_j)]$$

In QET/QSVT, the exact same template becomes:
$$U_{\text{QET/QSVT}} = e^{i\phi_0\Pi}\prod_j [U_A \, e^{i\phi_j\Pi}]$$

The invariant $SU(2)$ subspace of $U_A$ restricted to any eigenspace $|\psi_k\rangle$ looks exactly like single-qubit QSP, so the same phase sequence works. See [[Invariant SU(2) Subspace Embedding]].

## Circuit cost

- $O(m)$ gates for a single projector-controlled phase, where $m$ is the ancilla width
- Total circuit depth for degree-$d$ QSVT: $d$ applications of $U_A$ plus $O(d \cdot m)$ overhead gates
- For most practical block-encodings, $m = O(\log n)$ or similar, so this overhead is small

## Relation to circuits in the paper

Martyn et al. Fig. 3 shows the explicit implementation for a 2-qubit ancilla ($m=2$): a CNOT targeting a fresh qubit, then $R_z$ on that qubit, then CNOT to uncompute. Straightforward to generalize.

## Related notes

- [[QSP-to-QSVT Lifting Chain]] — why this gate is the right generalization
- [[QSVT Meta-Template]] — the full recipe using these gates
- [[One-Ancilla Alternating Phase Sequence]] — when ancilla is width 1
- [[Invariant SU(2) Subspace Embedding]] — the subspace structure that makes it work
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[grand-unification-projector-phase-circuit.excalidraw]]
