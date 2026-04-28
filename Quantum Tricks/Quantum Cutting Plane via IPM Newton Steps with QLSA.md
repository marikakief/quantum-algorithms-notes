# Quantum Cutting Plane via IPM Newton Steps with QLSA

> **Source:** Chakrabarti, Childs, Li & Wu, arXiv:1809.01731 (2020); builds on Vaidya (1989) volumetric center algorithm and [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] / [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma QLSA]]
> **Tags:** #trick #convex-optimization #cutting-plane #interior-point #QLSA #Newton-step #query-complexity

---

## What it does

Speeds up the cutting plane method for smooth convex optimization from $O(n^2)$ to $\tilde{O}(n^{3/2})$ gradient queries by replacing each Newton step computation (a linear system solve) with a quantum linear system solver.

---

## The trick

Classical cutting plane algorithms (e.g., Vaidya's volumetric center method) maintain a polytope $P_t = \{x : Ax \leq b\}$ that localizes the optimum, adding a new halfspace (gradient cut) at each iteration. The bottleneck is computing the **volumetric center** (or analytic center) of $P_t$, which requires a sequence of Newton steps. Each Newton step solves:
$$H_t \Delta x = -\nabla \phi(x_t)$$
where $H_t = \nabla^2 \phi(x_t)$ is the Hessian of the barrier function $\phi$ (a sum of $O(n)$ log-barrier terms after $O(n)$ cuts have been added). Classically, solving this costs $O(n^3)$ arithmetic or $O(n^\omega)$; across $O(n^2)$ total gradient queries, this gives a classical $O(n^2)$ gradient query count.

**Quantum speedup via QLSA.** Instead of classically inverting $H_t$, encode $H_t$ as a block-encoded matrix and apply the [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|quantum linear system solver]] to compute $|x^*\rangle \propto H_t^{-1} \nabla\phi(x_t)$:

1. **Block-encode $H_t$.** The barrier Hessian $H_t = \sum_{i=1}^{m} \frac{a_i a_i^T}{(a_i^T x - b_i)^2}$ is a sum of rank-1 outer products. Each term is efficiently block-encodeable; their sum is block-encoded via [[Block-Encoding Composition Algebra|LCU composition]].

2. **Apply QLSA.** With $H_t$ block-encoded, apply the QLSA to solve $H_t \Delta x = g$ in $\tilde{O}(\kappa(H_t) \sqrt{n})$ time (the $\sqrt{n}$ comes from a quantum speedup in the number of inner iterations).

3. **Extract the Newton step.** Measure the output state to obtain the direction $\Delta x$, then take the gradient step and add the new cutting halfspace.

The key saving: after $O(n)$ cuts have been added, $H_t$ is an $n \times n$ matrix with condition number $\kappa(H_t) = O(\text{poly}(n))$ (controlled by the barrier analysis). The QLSA costs $\tilde{O}(\kappa \cdot n^{1/2})$ per step — a $\sqrt{n}$ saving over the classical $O(n^2)$ (or $O(n^\omega)$) linear solve. Across $O(n^2)$ gradient queries, this reduces to $\tilde{O}(n^{3/2})$ total gradient queries.

**Coherence bookkeeping.** Since the QLSA outputs a quantum state rather than a classical vector, subsequent computations (gradient evaluation at the new center, updating the block-encoding of $H_t$) must be organized coherently or with controlled dequantization. The paper verifies that each stage can be done without breaking the quantum speedup.

---

## When to reach for it

- Smooth convex optimization over a polytope where the dimension $n$ is the bottleneck.
- When you have quantum access to both the gradient oracle and the constraint matrix encoding.
- The block-encoding of $H_t$ is efficiently constructible — this holds when the barrier is a sum of efficiently-queryable rank-1 or sparse terms.
- Prefer this over the [[Quantum Subgradient Method via Amplitude-Estimated Gradient Averaging|quantum subgradient method]] when $f$ is smooth and the geometry (polytope, condition number) is controlled.

---

## Complexity

$$\tilde{O}(n^{3/2}) \text{ quantum gradient queries}$$

vs. $O(n^2)$ classical (Vaidya). Lower bound from [[Convex Optimization Lower Bound via Search Embedding|search embedding]]: $\Omega(n)$, leaving an $\tilde{O}(\sqrt{n})$ gap.

---

## Caveat

- Requires quantum oracle access to the gradient and the ability to block-encode the running Hessian $H_t$. If the constraint matrix $A$ is dense and unstructured, constructing the block-encoding may dominate.
- QLSA gives a quantum **state** $|\Delta x\rangle$; extracting the classical Newton direction costs $O(n)$ measurements, negating the speedup for direct readout. The algorithm avoids this by using the state as input to the next quantum subroutine (oracle call), keeping computation coherent.
- The condition number $\kappa(H_t)$ must remain polynomial in $n$ throughout the algorithm. This follows from Vaidya's barrier analysis but requires careful potential function bookkeeping.
- Near-term hardware cannot maintain coherence across $\tilde{O}(n^{3/2})$ sequential quantum linear system solves; this is a fault-tolerant algorithm.

---

## Related notes

- [[Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Block-Encoding Composition Algebra]]
- [[QSVT Meta-Template]]
- [[Quantum Subgradient Method via Amplitude-Estimated Gradient Averaging]]
