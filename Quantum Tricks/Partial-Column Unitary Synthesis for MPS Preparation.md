# Partial-Column Unitary Synthesis for MPS Preparation

> **Source:** Berry, Tong, Khattar, White, Kim, Boixo, Lin, Lee, Chan, Babbush, Rubin, arXiv:2409.11748
> **Tags:** #trick #state-preparation #MPS #unitary-synthesis #QROM #fault-tolerant

## What it does

Cuts the cost of MPS preparation by compiling only the unitary columns that the circuit ever touches.

## The trick

In ancilla-based MPS preparation, each site tensor is embedded into a unitary on a physical register of dimension $d$ and a bond register of dimension $\chi$. But the physical input leg is fixed to $|0\rangle$, so one only needs the first $\chi$ columns of a $d\chi \times d\chi$ unitary, not the whole thing.

For the half-column case, write the required block as

$$
\begin{pmatrix}
A & ? \\
B & ?
\end{pmatrix}
=
\begin{pmatrix}
U_1 & 0 \\
0 & U_2
\end{pmatrix}
\begin{pmatrix}
D_1 & ? \\
D_2 & ?
\end{pmatrix}
\begin{pmatrix}
V & 0 \\
0 & V
\end{pmatrix},
$$

with $A = U_1 D_1 V$ and $B = U_2 D_2 V$. Then implement:

1. a unitary $V$ on the bond subspace,
2. a bond-controlled single-qubit rotation,
3. a control on that qubit selecting between $U_1$ and $U_2$.

For $d > 2$, reduce the $d\chi \times d\chi$ problem recursively to $d-1$ copies of the $d=2$ case by peeling off one physical sector at a time with QR decomposition.

The other half of the trick is to compile the needed subspace unitaries with alternating phase layers, Hadamards, and increment/decrement shifts, loading phase data through [[QROM (Quantum Read-Only Memory)]]. Consecutive MPS steps share internal subspace unitaries, so some of those pieces can be merged instead of paid for again.

## When to reach for it

- Preparing MPS or tensor-network states fault-tolerantly.
- Any state-preparation problem where the input is known to lie in a strict subspace, so only part of a unitary matters.
- Chemistry resource estimates where full generic unitary synthesis would dominate the state-prep bill.

## Complexity

The source paper reports about a $7\times$ Toffoli reduction over the Low-Kliuchnikov-Schaeffer synthesis route in the parameter regime relevant to chemistry MPS preparation. For physical dimension $d=4$, the improvement is still around $3.5\times$.

## Caveat

This is a constant-factor win, not a new asymptotic class. It helps because the MPS circuit has a lot of repeated structured synthesis. If the target unitary has no subspace restriction, or if the dimension is small enough that generic synthesis is already cheap, the gain shrinks.

## Related notes
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]]
- [[QROM (Quantum Read-Only Memory)]]
- [[Unary Iteration]]
