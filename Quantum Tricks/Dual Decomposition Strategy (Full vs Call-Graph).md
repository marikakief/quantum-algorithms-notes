> **Source:** Harrigan, Khattar, Yuan et al., arXiv:2409.04643
> **Tags:** #trick #software-pattern #resource-estimation #testing #compilation

## What it does
Allows a quantum subroutine to be specified at two levels of detail — full compute graph (with data flow) or lightweight call-graph (just callee list + counts) — and uses both as a cross-check for correctness.

## The trick
For any bloq, the author can provide:

1. **Full decomposition** — a compute graph specifying the exact data flow (which output wires of sub-bloq A connect to which input wires of sub-bloq B). Required for simulation and full circuit compilation.
2. **Call-graph decomposition** — an unstructured list of callees and their call counts, without specifying data flow. Sufficient for gate counting and qubit estimation.

The key design insight: most resource estimation work doesn't need full data flow. A researcher can quickly estimate costs by providing option (2) alone — even using literature-reported cost formulas for sub-bloqs that aren't yet fully implemented. This dramatically lowers the barrier to encoding a new algorithm.

When both decompositions are provided, Qualtran can automatically cross-check: derive a call graph from the full decomposition and compare it against the declared call-graph decomposition. Mismatches indicate bugs in either the implementation or the cost annotation.

**Additionally**, option (2) supports symbolic parameters that option (1) cannot: you can declare "this bloq calls X gate $n$ times" where $n$ is a SymPy variable, but you can't instantiate $n$ Python objects when $n$ is symbolic.

## When to reach for it
- Early-stage algorithm design: sketch the high-level structure with call-graph decompositions, defer full implementations
- Incremental refinement: start with cost estimates from literature, replace with full implementations as they're built
- Testing: once full implementations exist, the call-graph annotations serve as regression tests
- Hybrid approaches: full decomposition for the novel parts of your algorithm, call-graph for well-understood subroutines

## Complexity
No additional computational cost — the two decompositions are just different interfaces on the same object. The cross-check costs one additional call-graph traversal.

## Caveat
The call-graph decomposition can diverge from reality if the full implementation changes and the annotation isn't updated. This is the classic "comments get stale" problem from software engineering. Automated cross-checking mitigates but doesn't eliminate it.

## Related notes
- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]]
- [[Hierarchical Bloq Decomposition for Scalable Resource Counting]]
- [[Symbolic Cost Propagation Through Call Graphs]]
