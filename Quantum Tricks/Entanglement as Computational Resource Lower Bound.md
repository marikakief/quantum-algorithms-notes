# Entanglement as Computational Resource Lower Bound

> **Source:** R. Jozsa, N. Linden, arXiv:quant-ph/0201143
> **Tags:** #trick #entanglement #lower-bound #complexity #speedup

## What it does

Provides a necessary condition for exponential quantum speedup on pure states: the number of mutually entangled qubits must grow as $\omega(\log n)$ at some point during the computation.

## The trick

The argument is a contrapositive:

1. If the state at every step of a quantum algorithm is $p$-blocked for some fixed $p$, then the computation can be [[p-Blocked State Classical Simulation|efficiently classically simulated]] (Jozsa-Linden Theorem 1).
2. Therefore, if the quantum algorithm provides exponential speedup over all classical algorithms, the state cannot be $p$-blocked for any fixed $p$ — the number of mutually entangled qubits must grow without bound as the input size increases.

**Sharpening the bound:** If $p = O(\log n)$, a $p$-blocked state has $O(n \cdot 2^{O(\log n)}) = O(n \cdot \mathrm{poly}(n))$ parameters — still polynomial. So the simulation remains efficient. The threshold is:

- $p = O(\log n)$: classically simulable
- $p = \omega(\log n)$: simulation becomes superpolynomial; exponential speedup is possible

This means **exponential quantum speedup requires more than $O(\log n)$ qubits to be mutually entangled at some point in the computation**.

**Note:** This says nothing about *how much* entanglement (in terms of entanglement entropy or other measures). It's about the *number of parties* involved. Even enormous bipartite entanglement between every pair of qubits doesn't help if no more than $O(\log n)$ are simultaneously entangled.

## When to reach for it

- When someone claims a quantum algorithm provides exponential speedup: check whether the algorithm generates unbounded multi-partite entanglement
- When designing quantum algorithms or ansätze: if the structure limits entanglement to fixed-size blocks, it can't beat classical exponentially
- When evaluating distributed quantum computation: if each node has $\leq p$ qubits and communication is classical, the computation is $p$-blocked — no exponential speedup is possible
- When comparing to the [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes|entanglement-induced barren plateaus]] result: Jozsa-Linden says you *need* large entanglement for speedup; Ortiz Marrero-Kieferová-Wiebe says too much entanglement kills gradient signal. The useful regime for variational algorithms is bounded from both sides.

## Complexity

Not applicable — this is a proof technique / lower-bound argument, not an algorithm.

## Caveat

- **Pure states only.** The bound doesn't apply to mixed-state quantum computation. [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes|DQC1]] appears to achieve speedup with separable (unentangled) mixed states. Whether this is genuine remains open.
- **Entanglement is necessary but not sufficient.** The [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes|Gottesman-Knill theorem]] shows that massive multi-partite entanglement can be classically efficiently simulated if it arises from stabiliser circuits. Other resources (magic, non-Clifford operations) are also needed.
- **The bound is not constructive.** Showing that an algorithm requires $\omega(\log n)$-partite entanglement doesn't tell you *how much* entanglement or at *which step* — just that it must occur somewhere.

## Related notes
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes]]
- [[p-Blocked State Classical Simulation]]
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]]
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes]]
