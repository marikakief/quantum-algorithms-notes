# Algebraic Gate Design via Temperley-Lieb Representations

> **Source:** Aharonov, Jones & Landau, arXiv:quant-ph/0511096
> **Tags:** #trick #gate-design #Temperley-Lieb #representation-theory

## What it does

Encodes a combinatorial/algebraic problem directly into quantum gates, so the circuit depth equals the problem size — no simulation overhead.

## The trick

The Temperley-Lieb algebra $TL_n(d)$ has generators $E_1, \ldots, E_{n-1}$ satisfying simple local relations. The braid group $B_n$ maps into $TL_n$ via $\sigma_i \mapsto AE_i + A^{-1}I$.

The path model representation maps each $E_i$ to a matrix acting on paths of length $n$ on a line graph $G_k$. When $|A| = 1$ (i.e., at roots of unity), the $E_i$ are Hermitian and the braid generators become unitary — so they're valid quantum gates.

Each braid crossing maps to a 2-qubit gate (acting on adjacent path qubits $i$ and $i+1$). A braid with $m$ crossings becomes a circuit of depth $m$. No Trotterisation, no LCU decomposition, no ancillae — just apply the gates in sequence.

## When to reach for it

- Problems that have a natural algebraic/representation-theoretic formulation.
- When you can find a *unitary* representation of the algebra underlying your problem.
- Jones polynomial, Tutte polynomial, other knot/graph invariants that factor through TL or similar algebras.

## Complexity

Circuit depth = number of algebraic operations (crossings). Each gate is a 2-local operation on adjacent qubits. Total gate count is $O(m)$ for $m$ crossings.

## Caveat

Only works when the relevant representation is unitary (or can be made unitary at the cost of post-selection). For non-unitary representations, you pay an exponential overhead in success probability — see [[Non-Unitary Gate Implementation via Post-Selection]].

The approach is specific to problems with the right algebraic structure. It doesn't generalise to arbitrary computational problems the way Hamiltonian simulation or QSVT does.

## Related notes
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]]
- [[Markov Trace Uniqueness for Jones Polynomial Computation]]
