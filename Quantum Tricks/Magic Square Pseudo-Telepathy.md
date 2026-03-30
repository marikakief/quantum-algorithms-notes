# Magic Square Pseudo-Telepathy

> **Source:** Cleve, Høyer, Toner, Watrous, arXiv:quant-ph/0404076 (game formulation); Mermin (1990), Aravind (2002) (Pauli construction)
> **Tags:** #trick #nonlocal-games #pseudo-telepathy #pauli-observables #contextuality

## What it does
Produces perfectly correlated, deterministic measurement outcomes satisfying contradictory parity constraints — the simplest demonstration that entangled strategies can achieve outcomes impossible classically.

## The trick

Consider the $3 \times 3$ matrix of two-qubit Pauli observables:

$$\begin{pmatrix} \sigma_x \otimes \sigma_y & \sigma_y \otimes \sigma_x & \sigma_z \otimes \sigma_z \\ \sigma_y \otimes \sigma_z & \sigma_z \otimes \sigma_y & \sigma_x \otimes \sigma_x \\ \sigma_z \otimes \sigma_x & \sigma_x \otimes \sigma_z & \sigma_y \otimes \sigma_y \end{pmatrix}$$

**Key properties:**
1. Observables within each row/column **commute** (so they can be simultaneously measured)
2. Product of each row = $+I \otimes I$ (even parity)
3. Product of each column = $-I \otimes I$ (odd parity)

Alice and Bob share two copies of $|\psi^-\rangle = (|01\rangle - |10\rangle)/\sqrt{2}$. Since $|\psi^-\rangle$ is a $-1$ eigenvector of $\sigma_x \otimes \sigma_x$, $\sigma_y \otimes \sigma_y$, and $\sigma_z \otimes \sigma_z$, Alice and Bob's individual measurements on their respective halves always agree.

**The game:** Alice gets a row or column, Bob gets one entry from it. Alice outputs all three values (satisfying the parity constraint), Bob outputs one value. The quantum strategy wins with probability 1. Classically, $\omega_c = 17/18$ because no $3 \times 3$ binary matrix can have even-parity rows and odd-parity columns simultaneously.

## When to reach for it

- Simplest counterexample to oracularisation in multi-prover interactive proofs
- Breaking classical proof system soundness with entangled provers (the 3-SAT counterexample uses just 24 clauses encoding this square)
- Teaching the distinction between quantum contextuality and classical satisfiability
- Constructing device-independent protocols requiring verifiable quantum correlations

## Complexity

Requires 2 EPR pairs (4 qubits total, 2 per party). Measurements are single-qubit Pauli measurements on each half of each pair — trivial circuit depth.

## Caveat

This is a pseudo-telepathy game: the advantage is qualitative (perfect vs. imperfect), not a complexity-theoretic speedup. It works because the parity constraints are globally inconsistent but locally satisfiable — entanglement lets Alice and Bob satisfy each local constraint without committing to a global assignment. The trick doesn't generalise straightforwardly to larger structures without finding analogous commuting observable arrangements.

## Related notes
- [[Consequences and Limits of Nonlocal Strategies (Cleve-Hoyer-Toner-Watrous 2004) — Paper Notes]]
- [[Nonlocal Game Framework]]
- [[Tsirelson Correspondence for XOR Games]]
