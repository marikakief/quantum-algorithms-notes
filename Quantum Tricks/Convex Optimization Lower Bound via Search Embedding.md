# Convex Optimization Lower Bound via Search Embedding

> **Source:** Chakrabarti, Childs, Li & Wu, arXiv:1809.01731 (2020); lower bound reduction pattern builds on [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes|Ambainis adversary method]] and the $\Omega(\sqrt{N})$ quantum search lower bound
> **Tags:** #trick #lower-bound #convex-optimization #query-complexity #search-reduction #oracle-lower-bound

---

## What it does

Proves quantum query lower bounds for convex optimization by embedding an unstructured search problem into a convex function, so that finding the optimum requires identifying a hidden "marked" index — requiring $\Omega(\sqrt{N})$ queries by the quantum search lower bound.

---

## The trick

**The construction.** Fix $N$ candidate indices. For each $j \in [N]$, define a convex function $f_j : \mathbb{R}^n \to \mathbb{R}$ that is identical to a "base" convex function $f_0$ everywhere except in a small region around the $j$-th "feature" (e.g., a perturbed halfspace, a shifted supporting hyperplane, or a shifted linear piece). The perturbation is designed so that:

1. **Convexity is preserved.** $f_j$ is convex for every $j$.
2. **The optimum reveals $j$.** The minimizer $x^*_j$ of $f_j$ over the domain $K$ lies in the region near feature $j$, so any algorithm that outputs an $\varepsilon$-optimal point must have "found" $j$.
3. **Gradient queries are uninformative away from $j$.** Querying the gradient at any point $x$ that is not near feature $j$ returns the same value as querying $f_0$. Thus, only queries near feature $j$ distinguish $f_j$ from $f_0$.

**Reduction.** Given any quantum optimization algorithm $\mathcal{A}$ that finds an $\varepsilon$-optimal point using $T$ gradient queries, construct a quantum search algorithm $\mathcal{B}$ for the marked index $j$:
- $\mathcal{B}$ simulates $\mathcal{A}$ on the oracle $O_{f_j}$.
- When $\mathcal{A}$ outputs an $\varepsilon$-optimal point, $\mathcal{B}$ reads off the feature location $j$.
- The simulation is query-efficient: each gradient query to $f_j$ is simulated by one query to the boolean "is index $j$ the marked one?" oracle.

By the [[Adversary Method for Quantum Query Lower Bounds|quantum search lower bound]] (BBBV 1994 / Ambainis 2000), $\mathcal{B}$ requires $\Omega(\sqrt{N})$ queries, so $T = \Omega(\sqrt{N})$.

**Calibrating $N$ to match the problem parameters.** For the Lipschitz convex lower bound with $\varepsilon$-accuracy and $n$-dimensional domain:
- Take $N = \Theta(n / \varepsilon^2)$ candidate feature directions (discretizing the gradient oracle over the dual sphere).
- The construction yields $T = \Omega(\sqrt{N}) = \Omega(\sqrt{n}/\varepsilon)$.

For the smooth convex lower bound:
- Take $N = \Theta(n^2)$ candidate hyperplane perturbations.
- This gives $T = \Omega(\sqrt{N}) = \Omega(n)$.

**Why convexity doesn't leak information.** The key technical point is that the perturbation near feature $j$ is small enough that the function is convex everywhere and the gradient is Lipschitz, but the perturbation is localized so that querying anywhere other than the $j$-th region gives the same gradient as $f_0$. This is achieved by constructing $f_j$ as a "soft" piecewise-linear (or smooth) function that changes only in a ball of radius $\delta \ll 1/\sqrt{N}$ around one of $N$ grid points on the domain.

---

## When to reach for it

- You want to show that any quantum algorithm for a continuous optimization problem (convex, Lipschitz, smooth) requires $\Omega(\sqrt{N})$ oracle queries in some parameterization.
- The oracle is a gradient (or function value) oracle and the hard instances can be parameterized by a hidden index $j \in [N]$ that the optimizer must identify.
- The structure of the hard family is convex (so you can't use simpler combinatorial lower bound constructions directly).
- You want a tight lower bound matching a quantum algorithm that achieves $O(\sqrt{N})$ queries.

---

## Complexity

No algorithmic cost — this is a proof technique. The reduction has linear overhead: one optimization query simulates one search query.

The lower bound produced: $\Omega(\sqrt{N})$ queries for any quantum algorithm, where $N$ is the number of hard instances.

---

## Caveat

- Only applies in the **black-box oracle model** — if the optimizer has structural knowledge of $f$ beyond what the oracle provides (e.g., knows $f$ is quadratic), the lower bound may not apply.
- The construction requires convexity across all $N$ instances to be maintained. Designing a hard family that is simultaneously convex everywhere and informative only near the planted index requires care; the paper handles this via smooth bump functions.
- The lower bound does not rule out polynomial (non-quadratic) quantum improvements in other parameters, only in the query count as a function of $N$ (which maps to $n$ and $\varepsilon$).
- Gap: for the smooth case, the lower bound is $\Omega(n)$ while the algorithm achieves $\tilde{O}(n^{3/2})$ — the search embedding doesn't immediately give $\Omega(n^{3/2})$.

---

## Related notes

- [[Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020) — Paper Notes]]
- [[Adversary Method for Quantum Query Lower Bounds]]
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]
- [[Hybrid Argument Lower Bound for Quantum Search]]
- [[Quantum Subgradient Method via Amplitude-Estimated Gradient Averaging]]
- [[Quantum Cutting Plane via IPM Newton Steps with QLSA]]
