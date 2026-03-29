# Graph-to-TL-Product Mapping for Tutte Polynomial

> **Source:** Aharonov, Arad, Eban & Landau, arXiv:quant-ph/0702008
> **Tags:** #trick #Tutte-polynomial #Temperley-Lieb #graph-algorithms

## What it does

Expresses the Tutte polynomial of any planar graph as a trace over a product of Temperley-Lieb operators, enabling quantum computation via the TL representation framework.

## The trick

In the AJL algorithm for the Jones polynomial, a braid naturally gives a sequence of TL generators — one per crossing. For a general planar graph $G$, the construction is:

1. Fix a planar embedding of $G$.
2. Order the edges $e_1, \ldots, e_L$.
3. Associate each edge $e_i$ with a TL operator: for edge weight $v_e$, the operator is $v_e E_{f(e)} + I$ (or a generalisation depending on orientation), where $f(e)$ maps the edge to the appropriate TL generator index based on the planar embedding.
4. The Tutte polynomial is proportional to the Markov trace of the product $\prod_i (v_{e_i} E_{f(e_i)} + I)$.

The key insight: the TL algebra's pictorial calculus (cap-cup diagrams) naturally matches the local structure of a planar graph. Each edge contributes one factor, and the trace "closes up" the boundary, just as in the Jones polynomial case.

## When to reach for it

- Computing Tutte/Potts/Jones invariants of planar graphs via quantum algorithms.
- Extending braid-based quantum algorithms to non-braid combinatorial settings.
- Any problem where a planar graph polynomial factors through the Temperley-Lieb algebra.

## Complexity

The product has $L = |E|$ factors, so the circuit depth is $O(|E|)$. Each factor is a 2-local operation.

## Caveat

Restricted to planar graphs — the TL structure requires planarity. For non-planar graphs, more general algebraic frameworks (e.g., BMW algebra) would be needed.

The edge ordering and embedding can affect the operator norms (hence the approximation quality), though not the final answer.

## Related notes
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]]
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]]
- [[Algebraic Gate Design via Temperley-Lieb Representations]]
- [[Markov Trace Uniqueness for Jones Polynomial Computation]]
- [[Generalized Temperley-Lieb Algebra for Arbitrary Graphs]] — the algebraic framework that makes this mapping work for arbitrary planar graphs (not just braids)
