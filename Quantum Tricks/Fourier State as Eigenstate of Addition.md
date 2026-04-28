# Fourier State as Eigenstate of Addition

> **Source:** Draper, arXiv:quant-ph/0008033; applied in Kitaev-Shen-Vyalyi (2002), Gidney (2018)
> **Tags:** #trick #QFT #phase-kickback #adder #circuit-primitive

## What it does
The QFT state $|F(a)\rangle$ is an eigenstate of the addition operator: adding $b$ produces a global phase $e^{2\pi i a b / 2^n}$ on each Fourier-basis qubit, rather than changing the computational basis state. This eigenvalue property turns addition into phase kickback.

## The trick

The [[Quantum Fourier Transform Circuit|QFT]] maps $|a\rangle$ to the product state:

$$|F(a)\rangle = \bigotimes_{k=1}^{n} \frac{1}{\sqrt{2}}\left(|0\rangle + e^{2\pi i a / 2^k}|1\rangle\right)$$

The addition operation (a sequence of controlled-$R_j$ rotations conditioned on $b$) acts on each qubit by shifting the phase:

$$e^{2\pi i a/2^k} \to e^{2\pi i (a+b)/2^k}$$

If you control the addition on a qubit $|c\rangle$, the qubit picks up phase $e^{2\pi i c \cdot b / 2^k}$ — this is phase kickback. The Fourier-basis register acts as a "phase accumulator" that converts controlled addition into a phase on the control.

This is exactly the mechanism behind the [[Phase Gradient State for Controlled Rotations|phase gradient state]]: prepare a fixed Fourier state $|F\rangle$, then perform controlled additions into it. The Fourier state is unchanged (it is an eigenstate of addition), and the control qubit acquires the desired phase rotation.

## When to reach for it

- Building [[Phase Gradient Rotation Synthesis|phase gradient rotation synthesis]] circuits: the Fourier state is the resource that converts addition into rotation.
- Understanding why [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] works: the eigenvalue equation for QFT and addition is structurally identical to the eigenvalue equation for a unitary and its eigenstates.
- Designing arithmetic circuits that stay in the Fourier basis across multiple operations, avoiding repeated QFT/inverse-QFT pairs.

## Complexity

No additional cost beyond the addition itself. The eigenstate property is a mathematical fact about the QFT, not a circuit construction. The cost of exploiting it is the cost of the controlled addition: $O(n^2)$ controlled rotations (exact) or $O(n \log n)$ (approximate).

## Caveat

The eigenvalue equation is modular: addition is mod $2^n$. If the sum overflows, the phase wraps around. This is correct for modular arithmetic but must be handled if non-modular addition is needed.

## Related notes
- [[Addition on a Quantum Computer (Draper 2000) — Paper Notes]] — source paper
- [[Arithmetic in the Fourier Basis]] — the addition construction that uses this property
- [[Phase Gradient State for Controlled Rotations]] — direct application: a fixed Fourier state used as a rotation resource
- [[Phase Gradient Rotation Synthesis]] — downstream: rotation via controlled addition into the phase gradient state
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase estimation uses the same eigenvalue/phase-kickback structure
