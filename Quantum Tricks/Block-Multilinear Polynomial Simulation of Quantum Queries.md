
> **Source:** Aaronson & Ambainis, arXiv:1411.5729 (2015)
> **Tags:** #trick #simulation #query-complexity #polynomial-method #classical-simulation

## What it does
Shows that the acceptance probability of any $t$-query quantum algorithm is a degree-$2t$ multilinear polynomial in the input bits, and that this polynomial can be estimated classically by querying $O(N^{1-1/(2t)})$ random input coordinates — giving a general classical simulation of constant-query quantum algorithms.

## The trick

**Step 1 — Polynomial representation.** The acceptance probability $p(x)$ of a $t$-query quantum algorithm on $N$-bit input $x \in \{0,1\}^N$ is a multilinear polynomial of degree at most $2t$:
$$p(x) = \sum_{S \subseteq [N],\, |S| \leq 2t} \hat{p}(S) \prod_{i \in S} x_i$$
This is a standard fact from the polynomial method in query complexity (Beals et al. 2001). For $\pm 1$ inputs the same holds with the substitution $x_i \mapsto (1 - z_i)/2$.

**Step 2 — Block-multilinear decomposition.** Write $p(x) = \sum_{k=0}^{2t} p_k(x)$ where $p_k$ contains exactly the degree-$k$ terms. Each $p_k$ is a symmetric function of random subsets of coordinates. Sample $m$ coordinates uniformly at random: let $I \subseteq [N]$ be a random subset of size $m$. For each monomial $\prod_{i \in S} x_i$ with $|S| = k$, the probability that $S \subseteq I$ is $\binom{m}{k}/\binom{N}{k} \approx (m/N)^k$.

**Step 3 — Unbiased estimator and variance.** Construct the estimator:
$$\hat{p}(x_I) = \sum_{S \subseteq I} \hat{p}(S) \cdot \left(\frac{N}{m}\right)^{|S|} \prod_{i \in S} x_i$$
This is unbiased: $\mathbb{E}[\hat{p}(x_I)] = p(x)$. The variance is bounded by:
$$\text{Var}[\hat{p}] \leq O\!\left(\frac{N^{2t}}{m^{2t}} \cdot \|\hat{p}\|_2^2\right)$$
where $\|\hat{p}\|_2^2 = \sum_S \hat{p}(S)^2 \leq p(x)^2 \leq 1$. Setting $m = O(N^{1-1/(2t)})$ ensures the variance is $O(1)$, which suffices for constant-bias estimation (amplify with $O(1)$ repetitions).

**Step 4 — Non-adaptive queries.** The $m$ coordinates are chosen uniformly at random *before* seeing any query answers — the simulation is non-adaptive. This is much weaker than adaptive classical query access.

**Consequence:** For any $t$-query quantum algorithm, $O(N^{1-1/(2t)})$ random queries (non-adaptive) give a constant-bias classical simulation. Setting $t=1$: $O(N^{1/2})$ queries simulate any 1-query quantum algorithm. Setting $t = \log N$: $O(1)$ queries suffice (trivially, but also: for $t \geq \frac{1}{2}\log N$ the quantum advantage disappears entirely).

## When to reach for it

- Proving that constant-query quantum algorithms cannot have super-polynomial classical query lower bounds (the simulation gives an explicit upper bound $O(N^{1-1/(2t)})$ on how hard classical simulation can be).
- Showing that quantum advantage in query complexity is at most polynomial for small-query algorithms.
- Any setting where you want to argue "if the quantum algorithm only makes $t$ queries, then a classical algorithm can simulate it with $N^{1-\epsilon}$ queries for some $\epsilon > 0$."

## Complexity

| Quantum queries | Classical non-adaptive queries |
|---|---|
| $t = 1$ | $O(\sqrt{N})$ |
| $t = 2$ | $O(N^{3/4})$ |
| $t = 3$ | $O(N^{5/6})$ |
| $t = \frac{1}{2}\log N$ | $O(1)$ |

The classical simulation uses $\text{poly}(N)$ time (polynomial in the degree of the polynomial representation).

## Caveat
- Non-adaptive queries only. Adaptive classical query algorithms may do much better; this simulation argues you don't *need* adaptivity, not that it can't help.
- The simulation works for *query* complexity. It says nothing about time complexity; the polynomial evaluation may require exponential time in general.
- The degree-$2t$ polynomial representation is exact for quantum query algorithms but not for other computational models.
- For $t = \Omega(\log N)$, the simulation uses $O(1)$ queries (trivially), so the result is interesting only for small (constant or polylog) $t$.

## Related notes
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]]
- [[Gaussian Lifting for Query Lower Bounds]]
- [[Near-Orthogonality of Standard-Fourier Basis Pairs]]
- [[Query Magnitude Hybrid Argument for Oracle Lower Bounds]]
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]]
