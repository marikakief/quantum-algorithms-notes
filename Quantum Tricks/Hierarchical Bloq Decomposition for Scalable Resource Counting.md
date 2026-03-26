# Hierarchical Bloq Decomposition for Scalable Resource Counting

> **Source:** Harrigan, Khattar, Yuan et al., arXiv:2409.04643
> **Tags:** #trick #resource-estimation #compilation #software-pattern #scalability

## What it does
Enables gate and qubit counting for algorithms with $10^9$+ gates in seconds of classical compute, by traversing a hierarchical call graph rather than flattening to a full circuit.

## The trick
Represent a quantum algorithm as a tree of **bloqs** — each bloq decomposes into a small number (tens to thousands) of sub-bloqs. Resource counting walks the call graph (a DAG of caller→callee edges annotated with call counts), recursively multiplying costs. Three properties make this work:

1. **Cache reuse** — identical sub-bloqs (same parameters) share a single cost computation. A modular exponentiation with $n$ identical multiplications computes the multiplication cost once and multiplies by $n$.
2. **Never flatten** — the classical memory footprint is proportional to the depth × branching factor of the hierarchy, not the total gate count. A 2048-bit modular exponentiation (billions of gates) fits in the same Python object as a 4-bit one.
3. **Symbolic propagation** — sub-bloq costs can be SymPy expressions. The call graph multiplies and sums symbolic expressions, producing a closed-form cost formula for the whole algorithm.

This is the same idea as memoized recursion in classical algorithms, applied to quantum circuit analysis. The key insight is that quantum algorithms are highly structured — the same subroutine appears thousands of times with the same parameters — so caching is extremely effective.

## When to reach for it
- You're estimating resources for a fault-tolerant algorithm and the full circuit would have $> 10^6$ gates
- You need to compare algorithms parametrically (varying basis size, error tolerance, etc.)
- You want to identify which subroutine dominates the total cost — call-graph traversal naturally attributes costs to each node

## Complexity
Classical time: proportional to the number of *distinct* bloqs in the hierarchy (typically $O(D \cdot B)$ where $D$ is depth and $B$ is max branching factor). Classical space: same order.

## Caveat
Qubit counting via this method assumes sequential execution of sub-bloqs, which overestimates the total qubit count. More aggressive parallelism-aware scheduling could reduce qubit counts but would require flattening parts of the hierarchy. The tradeoff between resource counting speed and qubit count accuracy is inherent.

## Related notes
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]]
- [[Symbolic Cost Propagation Through Call Graphs]]
- [[Dual Decomposition Strategy (Full vs Call-Graph)]]
