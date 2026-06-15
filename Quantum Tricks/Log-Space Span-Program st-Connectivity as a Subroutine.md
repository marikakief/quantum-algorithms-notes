# Log-Space Span-Program st-Connectivity as a Subroutine

> **Source:** Belovs--Reichardt via Cade--Montanaro--Belovs, arXiv:1610.00581
> **Tags:** #trick #span-programs #st-connectivity #space-efficient #graph-algorithms

## What it does

Uses the Belovs--Reichardt span-program algorithm for $s$-$t$ connectivity as a time-efficient, logarithmic-space subroutine when the graph is implicit.

## The theorem used

For an $N$-vertex graph with the promise that any positive instance has an $s$-$t$ path of length at most $d$, $s$-$t$ connectivity can be decided in
$$
\widetilde O(N\sqrt d)
$$
time using $O(\log N)$ bits/qubits of storage.

The span program uses target vector $|t\rangle-|s\rangle$ and input vectors $|v\rangle-|u\rangle$ for graph edges. A positive path gives a telescoping positive witness; a disconnected cut gives a negative witness.

## Implementation point

The hard reflection about the span-program kernel is implemented by a Szegedy-type walk on a factorisation of the span-program matrix. For the complete potential-edge structure, the relevant Laplacian has a constant nonzero spectral gap, so the reflection costs only polylogarithmic overhead beyond local operations.

## When to use it

- When you can query an implicit graph locally but cannot store it.
- Reductions where witnesses become short paths.
- Space-sensitive graph-property algorithms.

## Related notes

- [[Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016) — Paper Notes]]
- [[Mod-k Lift from Cycle Detection to st-Connectivity]]
