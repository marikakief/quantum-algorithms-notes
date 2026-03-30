# Alphabet Reduction to n for Schur Transform

> **Source:** Hari Krovi, arXiv:1804.00055
> **Tags:** #trick #representation-theory #Schur-transform #encoding

## What it does
Removes the dependence on the local dimension $d$ from the Schur transform by mapping the $d$-letter alphabet to at most $n$ letters.

## The trick
An $n$-tuple $(e_1, \ldots, e_n)$ with $e_i \in [d]$ can use at most $n$ distinct values. So:

1. Identify the distinct values appearing in the tuple and assign them labels in $[n]$.
2. Record the mapping $p_e : [n] \to [d]$ that remembers which original labels correspond to which compressed labels.
3. All subsequent processing (type extraction, transversal computation, QFT over $S_n$, GPE) works on the compressed tuple with entries in $[n]$.
4. At the end, use $p_e$ and the RSK algorithm to recover the correct SSYT with entries in $[d]$.

The mapping is reversible and local to each basis vector, so it can be done coherently.

## When to reach for it
Any time you have a tensor product $V^{\otimes n}$ where $\dim(V) = d \gg n$ and need to exploit symmetric-group structure. The trick reduces the effective alphabet from $d$ to $n$, making all $S_n$-based algorithms dimension-independent.

## Complexity
$O(\mathrm{poly}(n, \log d))$ for the mapping step. The $\log d$ comes from reading and comparing the register values.

## Caveat
Only useful when $d > n$. For $d \leq n$ (the qubit/low-dimensional case), this step is unnecessary. The RSK recovery step at the end adds polynomial overhead.

## Related notes
- [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes]]
- [[QFT over Permutation Modules]]
