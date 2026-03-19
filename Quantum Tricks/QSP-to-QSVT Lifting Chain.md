# QSP-to-QSVT Lifting Chain

> **Source:** Martyn, Rossi, Tan & Chuang, *Grand Unification of Quantum Algorithms*, PRX Quantum **2**, 040203 (2021) — Sections II–IV.
> **Tags:** #trick #QSP #QET #QSVT #conceptual-chain #block-encoding

## What it is

A three-step conceptual chain that shows how single-qubit QSP generalizes naturally to arbitrary matrix polynomial transforms. Martyn et al. introduce this as the pedagogical structure of their paper, and it's the cleanest way to understand why QSVT looks the way it does.

```
Level 1: QSP
  - Input: scalar a ∈ [-1,1]
  - Object: single qubit
  - Signal: W(a) rotation
  - Phase: S(φ) = diag(e^{iφ}, e^{-iφ})
  - Output: polynomial P(a) in top-left entry

Level 2: QET (Quantum Eigenvalue Transform)
  - Input: Hermitian H, block-encoded as U_H
  - Object: multi-qubit + ancilla
  - Signal: U_H (block-encoding)
  - Phase: e^{iφΠ} (projector-controlled phase)
  - Output: polynomial P(H/α) applied to eigenvalues

Level 3: QSVT (Quantum Singular Value Transformation)
  - Input: general A, block-encoded as U_A
  - Object: multi-qubit + two-sided ancilla
  - Signal: U_A and U_A† (alternating)
  - Phase: e^{iφΠ} and e^{iφΠ̃} (left and right projectors)
  - Output: polynomial P applied to singular values of A/α
```

## Each generalization step

**QSP → QET:**  
Replace the scalar $a$ with the block-encoding $U_H$ of a Hermitian $H/\alpha$. The single-qubit phase $S(\phi)$ becomes a [[Projector-Controlled Phase Gate]] $e^{i\phi\Pi}$. This works because $U_H$ preserves the [[Invariant SU(2) Subspace Embedding|invariant 2D subspaces]] $\{|\tilde{0}\rangle|\psi_k\rangle, \ldots\}$ for each eigenstate $|\psi_k\rangle$. Inside each such subspace, $U_H$ acts like a single-qubit rotation with angle $\lambda_k/\alpha$ — same structure as $W(a)$ with $a = \lambda_k/\alpha$.

**QET → QSVT:**  
Hermitian $H$ → general $A$ (non-Hermitian, possibly rectangular). For singular values, $U_A$ and $U_A^\dagger$ together span the needed SU(2) structure on both the left and right singular vector spaces. Use two projectors: $\Pi$ (left ancilla) and $\tilde\Pi$ (right ancilla). The alternating $U_A, U_A^\dagger$ circuit creates the invariant SU(2) subspaces on singular values rather than eigenvalues.

## Why this structure

Without the intermediate QET step, the leap from QSP to QSVT looks like: "we added block-encoding and now it works for matrices." With QET, you see the logical progression:

1. Single-qubit phases work because the signal rotation lives in $SU(2)$
2. Block-encoding of Hermitian embeds eigenvalues into the same $SU(2)$ structure  
3. Extending to general matrices requires both $U_A$ and $U_A^\dagger$ to recover the $SU(2)$ structure on singular values

Each step preserves the core algebraic structure; only the domain of "what the signal represents" changes.

## Practical consequence

The **same phase angles $\vec\phi$** (satisfying the QSP achievability conditions for polynomial $P$) work at all three levels. You don't redesign the phase sequence for different problems — you just change what the block-encoding encodes. The polynomial approximation step is entirely classical.

## What it doesn't tell you

This is a conceptual guide to structure, not an algorithm on its own. The hard parts are:
- Finding the phase angles $\vec\phi$ (classical computation, numerically non-trivial)
- Building the block-encoding (problem-dependent, often the dominant cost)
- Designing the polynomial $P$ (approximation theory, parity constraints)

See [[QSVT Meta-Template]] for the full practical recipe.

## Related notes

- [[Projector-Controlled Phase Gate]] — the technical bridge between levels
- [[Invariant SU(2) Subspace Embedding]] — why each level preserves the SU(2) structure
- [[QSVT Meta-Template]] — the operational recipe
- [[Parity-Aware Polynomial Design]] — polynomial constraints
- [[One-Ancilla Alternating Phase Sequence]] — circuit realization
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[grand-unification-qsp-to-qsvt-hierarchy.excalidraw]]
