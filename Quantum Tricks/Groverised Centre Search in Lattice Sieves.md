# Groverised Centre Search in Lattice Sieves

> **Source:** Laarhoven, Mosca, van de Pol, arXiv:1301.6176
> **Tags:** #trick #quantum-search #lattice-sieving #shortest-vector-problem

## What it does

Turns the centre lookup inside a lattice sieve from a linear scan into a square-root search while leaving the sieve logic unchanged.

## The trick

Many lattice sieves maintain a list $C$ of centres and, for every vector $v$ in a working list $S$, ask whether there is a centre satisfying a geometric predicate such as

$$
\|v-c\| \leq \gamma R.
$$

Classically this costs $O(|C|)$ per vector, so the round costs roughly

$$
\widetilde{O}(|S||C|).
$$

If the list $C$ is indexed and the distance predicate is reversible, use [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] over $C$ instead. The cost becomes

$$
\widetilde{O}(|S|\sqrt{|C|}).
$$

The outer sieve does not need to be re-derived: if the classical analysis bounds $|S|$ and $|C|$ and only charges for the centre lookup, the quantum exponent follows by halving the centre-list contribution.

For the Nguyen--Vidick sieve analysed by Laarhoven--Mosca--van de Pol, $|S|,|C|\leq 2^{c_h n+o(n)}$, where

$$
c_h=-\log_2(\gamma)-\frac12\log_2\left(1-\frac{\gamma^2}{4}\right).
$$

The classical lookup cost $2^{2c_hn+o(n)}$ becomes $2^{(3c_h/2)n+o(n)}$.

## When to reach for it

Use this when a classical geometric or algebraic algorithm repeatedly asks:

1. maintain a large list of candidate representatives;
2. for each new object, find any representative satisfying a simple predicate;
3. the list behaviour does not depend on the search path, only on whether a matching representative exists.

Lattice sieves are the paper's example, but the pattern also applies to nearest-centre reductions, meet-in-the-middle filters, and list-decoding-style algorithms where the predicate is cheap relative to list size.

## Complexity

If the classical cost is $\widetilde{O}(N M)$ for $N$ outer items and a searched list of size $M$, the Groverised cost is

$$
\widetilde{O}(N\sqrt{M})
$$

plus the cost of reversible predicate evaluation and indexed list access.

## Caveat

The list must be available through coherent indexed lookup, typically [[QRACM Accounting for Quantum Search over Classical Lists|quantumly addressable classical memory]]. If fetching the $j$th centre costs more than polylogarithmic time or cannot be queried in superposition, the square-root gain may not translate into a real resource gain.

## Related notes

- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]]
- [[Quantum Birthday Search in Saturation SVP]]
- [[QRACM Accounting for Quantum Search over Classical Lists]]
- [[Standard Amplitude Amplification]]
