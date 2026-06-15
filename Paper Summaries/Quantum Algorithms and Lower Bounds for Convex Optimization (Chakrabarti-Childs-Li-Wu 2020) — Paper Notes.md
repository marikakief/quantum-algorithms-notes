# Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020) — Paper Notes

> **Source:** Shouvanik Chakrabarti, Andrew M. Childs, Tongyang Li, Xiaodi Wu, "Quantum algorithms and lower bounds for convex optimization," *Quantum* **4**, 221 (2020); arXiv:1809.01731
> **Links:** [arXiv](https://arxiv.org/abs/1809.01731) · [Quantum journal](https://doi.org/10.22331/q-2020-01-13-221)
> **Tags:** #convex-optimization #quantum-algorithms #lower-bounds #query-complexity #membership-oracle #evaluation-oracle

---

## The computational problem

The paper studies black-box convex optimization over an $n$-dimensional convex body $K \subset \mathbb{R}^n$.

Input access is by:

- an **evaluation oracle** for a convex objective $f$;
- a **membership oracle** for the feasible convex body $K$;
- standard geometric promises, such as bounded radii and bounded objective range, so that approximation and finite precision are meaningful.

The goal is to output a point whose objective value is within the required additive accuracy of the optimum, while minimizing the number of oracle queries.

This is a query-complexity result. It is not a statement about a particular gradient method, matrix solver, or coherent implementation of an optimization package.

---

## What the paper does

The headline result is a quantum algorithm using
$$
\tilde O(n)
$$
evaluation and membership queries for $n$-dimensional convex optimization. This gives a quadratic improvement over the best-known classical query bounds in the same black-box model.

The paper also proves lower bounds:

- any quantum algorithm needs $\tilde\Omega(\sqrt n)$ evaluation queries in the relevant model;
- any quantum algorithm needs $\Omega(\sqrt n)$ membership queries.

So the paper establishes a genuine quantum query speedup for convex optimization, but it does not close the gap between $\tilde O(n)$ and the known lower bounds.

An independent work by van Apeldoorn, Gilyén, Gribling, and de Wolf obtained closely related quantum convex-optimization results; this paper notes that parallel development.

---

## Algorithmic picture

The algorithm combines classical convex-geometric reductions with quantum search-style primitives.

1. Reduce convex optimization to a sequence of geometric oracle tasks over well-controlled regions of the convex body.
2. Use quantum search/minimum-finding ideas to reduce the number of candidate evaluations needed in those tasks.
3. Use membership access to keep the geometric search inside the feasible body.
4. Manage precision and rounding so that the extra dependence is hidden in logarithmic factors.

The important model point is that the query improvement is over objective-evaluation and membership access. The paper's theorem should not be paraphrased as a speedup for first-order methods or for solving linear systems inside a continuous optimizer.

---

## Key results

| Result | Statement |
|---|---|
| Quantum upper bound | $\tilde O(n)$ evaluation/membership queries for $n$-dimensional convex optimization |
| Evaluation lower bound | $\tilde\Omega(\sqrt n)$ quantum evaluation queries |
| Membership lower bound | $\Omega(\sqrt n)$ quantum membership queries |
| Classical comparison | Quadratic improvement over best-known classical query bounds in the same oracle model |

The lower bounds are search-style oracle lower bounds: the hard instances hide information in the black-box access so that finding an approximate optimum reveals a searched-for object.

---

## Comparison with prior work

| Work / model | Role |
|---|---|
| Classical black-box convex-body optimization | Baseline query model for the quadratic improvement |
| Grover search and quantum minimum finding | Conceptual source of the quadratic query improvement |
| Quantum query lower bounds for search | Source of the $\sqrt n$-type lower-bound constructions |
| van Apeldoorn-Gilyén-Gribling-de Wolf | Independent related quantum convex-optimization result |

---

## Limits / caveats

- **Query complexity only.** The result counts oracle calls. A full implementation must account for finite precision, arithmetic, data representation, and the cost of realizing the oracles.
- **Oracle model matters.** Evaluation, membership, separation, gradient, and Hessian access are different resources. This paper's main theorem is about evaluation and membership oracles.
- **Lower bounds do not match the upper bound.** The upper bound is $\tilde O(n)$, while the lower bounds are around $\sqrt n$.
- **Structured optimization can behave differently.** Additional algebraic, geometric, or smoothness structure may change the best classical and quantum approaches.

---

## Reusable ideas

1. [[Convex Optimization Lower Bound via Search Embedding]] — encode a search problem into a black-box convex-optimization instance.
2. Quantum search over geometric candidates — use quantum search/minimum-finding primitives inside convex-geometric reductions.
3. Membership-oracle navigation — treat feasible-set access as a separate black-box resource from objective evaluation.

---

## References within this paper

- Grover-style quantum search lower bounds and minimum finding — the query-complexity engine behind the speedup and lower bounds.
- Classical convex-geometry algorithms for black-box optimization — the baseline model being improved.
- van Apeldoorn, Gilyén, Gribling, and de Wolf — independent related quantum convex-optimization work.

---

## Cross-links

### Paper notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]

### Trick cards

- [[Convex Optimization Lower Bound via Search Embedding]]
