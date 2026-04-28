# Quantum Subgradient Method via Amplitude-Estimated Gradient Averaging

> **Source:** Chakrabarti, Childs, Li & Wu, arXiv:1809.01731 (2020); builds on Jordan (2005) gradient estimation and [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|BHMT amplitude estimation]]
> **Tags:** #trick #convex-optimization #subgradient #amplitude-estimation #query-complexity

---

## What it does

Performs subgradient descent on a Lipschitz convex function using $O(n^{1/2} / \varepsilon)$ quantum gradient oracle queries, achieving a quadratic speedup over the classical $O(n / \varepsilon^2)$ bound in both dimension and accuracy.

---

## The trick

**Step 1 — Jordan gradient oracle.** For a smooth function $f : \mathbb{R}^n \to \mathbb{R}$, Jordan's 2005 algorithm estimates $\nabla f(x)$ using a single quantum superposition query over all $2^n$ sign combinations:
$$\nabla_i f(x) \approx \frac{1}{2\delta} \sum_{s \in \{0,1\}^n} (-1)^{s_i} f\!\left(x + \delta \sum_j (-1)^{s_j} e_j\right)$$
This costs $O(1)$ quantum evaluation queries (independent of $n$) rather than $O(n)$ finite-difference queries classically. The trick is that querying $f$ in superposition over all $2^n$ sign patterns yields all $n$ partial derivatives simultaneously via interference.

**Step 2 — Amplitude-estimated gradient averaging.** Classical subgradient descent averages $T$ gradient steps; the iterate average satisfies
$$f(\bar{x}) - f(x^*) \leq \frac{GD}{\sqrt{T}}$$
for $G$-Lipschitz $f$ over a $D$-diameter domain, requiring $T = O(G^2 D^2 / \varepsilon^2)$ gradient calls.

Instead, encode the subgradient descent trajectory into a quantum circuit and use [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude estimation]] to estimate the quantity $f(\bar{x}) - f(x^*)$ with $O(\sqrt{T})$ circuit calls. Specifically:
- Construct a unitary $U$ that applies one subgradient step: $|t, x\rangle \mapsto |t+1, x - \eta_t g_t\rangle$.
- Apply $U^T$ in superposition and use amplitude estimation on the observable "is $f(\bar{x}) \leq f(x^*) + \varepsilon$?".
- The amplitude is $\Omega(1)$ when $T$ is large enough, so $O(\sqrt{T})$ applications of $U$ (each costing $O(1)$ gradient queries) suffice.

Total query count: $O(\sqrt{T}) \cdot O(1) = O(1/\varepsilon)$ for fixed $n$, or $O(n^{1/2}/\varepsilon)$ counting the Jordan speedup in dimension.

**Step 3 — Projection.** Each iterate $x_{t+1} = \text{proj}_K(x_t - \eta_t g_t)$ requires a projection oracle for the convex set $K$. In the quantum algorithm this must be computable reversibly (or via quantum access to the projection), which is standard for polytopes and balls.

---

## When to reach for it

- Minimizing a convex, $G$-Lipschitz function over a compact convex set when you have quantum oracle access to gradients/subgradients.
- When the classical bottleneck is the number of gradient evaluations and $n$ is large.
- When you want $\varepsilon$-accuracy and $\varepsilon$ is small — the $1/\varepsilon$ dependence (vs classical $1/\varepsilon^2$) gives the main speedup in high-accuracy regimes.
- Does **not** help when $f$ is smooth and the classical $O(1/\sqrt{\varepsilon})$ Nesterov accelerated gradient already dominates.

---

## Complexity

$$Q = O\!\left(\frac{n^{1/2} G D}{\varepsilon}\right) \text{ quantum gradient queries}$$

Matching lower bound: $\Omega(n^{1/2} / \varepsilon)$ from the [[Convex Optimization Lower Bound via Search Embedding|search embedding lower bound]].

---

## Caveat

- Requires **quantum access** to the gradient oracle — the oracle must be evaluatable coherently on superpositions of input points.
- Jordan's gradient estimation requires $f$ to be sufficiently smooth (bounded derivatives of appropriate order); for non-smooth Lipschitz functions, a regularized variant is needed.
- The speedup in $\varepsilon$ (from $\varepsilon^{-2}$ to $\varepsilon^{-1}$) uses amplitude estimation on the iterate average, which requires the descent trajectory to be reversibly computable — memory cost scales with the number of steps.
- For smooth functions, the Nesterov optimal rate $O(1/\sqrt{\varepsilon})$ classically already beats this in the $\varepsilon$ parameter (though the dimension dependence still favors quantum).

---

## Related notes

- [[Quantum Algorithms and Lower Bounds for Convex Optimization (Chakrabarti-Childs-Li-Wu 2020) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Convex Optimization Lower Bound via Search Embedding]]
- [[Quantum Cutting Plane via IPM Newton Steps with QLSA]]
