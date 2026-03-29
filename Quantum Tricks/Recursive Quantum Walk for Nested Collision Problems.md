# Recursive Quantum Walk for Nested Collision Problems

> **Source:** Magniez, Santha, Szegedy, arXiv:quant-ph/0310134
> **Tags:** #trick #quantum-walk #element-distinctness #triangle #recursion #query-complexity

## What it does

Solves problems where detecting a "collision" in a subset requires solving a structured sub-problem, by nesting one quantum walk inside another. The outer walk explores subsets of the input; the inner walk performs the checking step.

## The trick

The [[Walk on the Johnson Graph for Subset Search|Generic Algorithm]] (Ambainis's quantum walk framework) has three costs:
- **Setup** $s(r)$: load data for an $r$-element subset
- **Update** $u(r)$: swap one element in/out
- **Checking** $c(r)$: determine if the current subset contains a collision

For element distinctness, $c(r) = 0$ because collisions are internal to the stored data. But for Triangle, checking whether a subset $A$ of vertices participates in a triangle requires finding a vertex $v \notin A$ that connects to an edge in $G|_A$. This is itself a structured search problem.

**The recursive step:** The checking procedure for the outer walk runs:
1. Grover search over candidate vertices $v \in [n]$: cost $O(\sqrt{n})$ iterations
2. For each $v$, solve **Graph Collision** on $G|_A$ — does $A$ contain an edge $(u, u')$ with both $u, u'$ adjacent to $v$? This is an inner quantum walk on subsets of $A$, costing $\tilde{O}(r^{2/3})$

Total checking cost: $c(r) = \tilde{O}(\sqrt{n} \cdot r^{2/3})$

Plugging into the Generic Algorithm formula:
$$\tilde{O}\left(s(r) + \frac{n}{r}\left(c(r) + \sqrt{r} \cdot u(r)\right)\right) = \tilde{O}\left(r^2 + \frac{n}{r}\left(\sqrt{n} \cdot r^{2/3} + \sqrt{r} \cdot r\right)\right)$$

Optimised at $r = n^{3/5}$, this gives $\tilde{O}(n^{13/10})$.

## When to reach for it

- Any problem where the natural collision relation requires solving a sub-problem that is itself a collision/search problem
- Finding copies of a fixed subgraph $H$ in a graph: the outer walk maintains a subset of vertices, the inner walk checks for $H$-structure
- More generally, whenever the checking subroutine of a quantum walk has enough structure to admit its own quantum speedup

## Complexity

For $k$-vertex subgraph finding: $\tilde{O}(n^{2-2/k})$ queries, optimised at $r = n^{1-1/k}$.

For Triangle ($k = 3$): $\tilde{O}(n^{13/10})$, later improved to $\tilde{O}(n^{5/4})$ by Le Gall using non-uniform data structures.

## Caveat

- The recursion depth here is only 2 (outer walk + inner walk). Deeper nesting is possible in principle but the overhead compounds.
- The inner walk (Graph Collision) works on the *known* subgraph $G|_A$ and queries only the oracle function $f(u) = [(u, v) \in G]$. The structure of the known graph is what enables the $\tilde{O}(r^{2/3})$ cost rather than the naive $\tilde{O}(r)$.
- Polylogarithmic factors are hidden throughout.

## Related notes
- [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes]]
- [[Walk on the Johnson Graph for Subset Search]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
