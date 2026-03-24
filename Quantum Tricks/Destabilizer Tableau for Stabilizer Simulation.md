# Destabilizer Tableau for Stabilizer Simulation

> **Source:** Aaronson & Gottesman, arXiv:quant-ph/0406196 (2004)
> **Tags:** #trick #stabilizer #tableau #classical-simulation #clifford

## What it does

Stores $n$ **destabilizer** generators alongside $n$ **stabilizer** generators, so that finding the right pivot row for a measurement requires $O(1)$ lookup instead of $O(n^2)$ Gaussian elimination. Reduces measurement cost from $O(n^3)$ to $O(n^2)$.

## The trick

The stabilizer state $|\psi\rangle$ is represented by a $2n \times (2n+1)$ binary tableau:
- Rows $1\ldots n$: destabilizer generators $D_1, \ldots, D_n$
- Rows $n+1\ldots 2n$: stabilizer generators $G_1, \ldots, G_n$

For each row $i$ and qubit $j$, store bits $(x_{ij}, z_{ij})$ encoding the Pauli:
$(0,0) = I$, $(1,0) = X$, $(1,1) = Y$, $(0,1) = Z$.
Plus a phase bit $r_i$.

**Invariants maintained by all gate updates:**
1. Stabilizer rows generate $S(|\psi\rangle)$; all $2n$ rows generate $\mathcal{P}_n$.
2. Destabilizers commute with each other.
3. $D_i$ anticommutes with $G_i$ (and only $G_i$ among stabilizers).
4. $D_i$ commutes with $G_j$ for $i \neq j$.

**Measurement of qubit $a$:** Scan stabilizer rows for one with $x_{pa} = 1$ (random-outcome case). Because of invariant (3), the corresponding destabilizer $D_{p-n}$ is the unique destabilizer anticommuting with the measurement operator $Z_a$. No elimination needed — the pivot is found directly.

The original Gottesman-Knill representation stored only stabilizer generators. To determine the measurement outcome deterministically (case II), it needed to bring the tableau to reduced row echelon form: $O(n^2)$ rowsum operations, each $O(n)$, giving $O(n^3)$ total. The destabilizer representation replaces this with $O(n)$ rowsums for the random-outcome case, and a single scratch-row computation for the deterministic case — both $O(n^2)$.

**Memory cost:** The $2n$ rows (instead of $n$) exactly double the bit storage. For $n$ qubits: $\approx 4n^2$ bits.

## When to reach for it

- Simulating Clifford/stabilizer circuits classically, especially when measurements are frequent.
- Any setting where you have a group-theoretic representation and need efficient pivot-finding without elimination.
- Building stabilizer-based noise models or error-correction simulators.

## Complexity

- Unitary gate (CNOT, $H$, $S$): $O(n)$ time per gate.
- Measurement: $O(n^2)$ time worst case.
- Space: $O(n^2)$ bits.

## Caveat

The factor-of-2 memory overhead is unavoidable with this representation. For very large $n$ (many thousands of qubits), memory can dominate before gate count does. Also, the $O(n^2)$ measurement bound is worst-case; for random circuits below the $\Theta(n \log n)$-gate phase transition, measurements are much cheaper in practice.

## Related notes
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
- [[Rowsum Phase-Tracking in Pauli Group Arithmetic]]
- [[Stabilizer Canonical Form via Gate Type Sorting]]
