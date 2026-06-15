# Parity Gadget Lower Bound for Array-Model Graph Properties

> **Source:** Cade--Montanaro--Belovs, arXiv:1610.00581; Dürr--Heiligman--Høyer--Mhalla style reduction
> **Tags:** #trick #lower-bound #parity #graph-property-testing #adjacency-array

## What it does

Reduces parity to graph properties in the adjacency-array model, giving an $\Omega(n)$ quantum query lower bound.

## The trick

Given $x\in\{0,1\}^p$, create a two-level graph with vertices $v_{i,b}$ for column $i$ and level $b\in\{0,1\}$. Edges connect column $i$ to $i+1$, swapping levels iff $x_i=1$.

Following the graph around accumulates the parity of $x$:

- even parity returns to the starting level;
- odd parity switches level.

For cycle detection, remove one edge so the graph has a cycle iff the parity has the desired value. For bipartiteness, add or omit a dummy bit so odd cycles appear exactly in one parity case.

Since quantum parity needs $\Omega(p)$ queries, the graph property needs $\Omega(n)$ adjacency-array queries.

## When to use it

- Lower bounds for adjacency-array graph problems.
- Separating matrix-model and array-model behaviour.
- Encoding a global bit property into cycle structure.

## Related notes

- [[Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016) — Paper Notes]]
- [[Frequency-Moment Lower Bounds by Query and Communication Reductions]]
