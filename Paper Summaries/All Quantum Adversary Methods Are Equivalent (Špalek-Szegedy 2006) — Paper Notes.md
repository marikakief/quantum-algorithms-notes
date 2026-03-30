> **Source:** Robert Špalek and Mario Szegedy, "All Quantum Adversary Methods Are Equivalent", *Theory of Computing* 2:1–18, 2006 (originally ICALP 2005)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0409116) · [Journal](http://theoryofcomputing.org/articles/v002a001/)
> **Tags:** #query-complexity #lower-bounds #adversary-method #semidefinite-programming #quantum-complexity

---

## The problem

In quantum query complexity, the input is accessed through oracle queries and we want to lower-bound the number of queries needed to compute a function $f : S \to H$ with bounded error. By 2004, several variants of the *quantum adversary method* had emerged for proving such lower bounds, each with different formulations and seemingly different strengths:

1. **Unweighted adversary** (Ambainis 2000) — choose pairs $(x,y)$ with $f(x) \neq f(y)$; bound based on combinatorial properties of this set
2. **Spectral adversary** (Barnum-Saks-Szegedy 2003) — uses spectral norms of adversary matrices $\Gamma$
3. **Weighted adversary** (Ambainis 2003) — assigns non-negative weights to input pairs
4. **Strong weighted adversary** (Zhang 2005) — unifies Ambainis's two previous methods
5. **Kolmogorov complexity adversary** (Laplante-Magniez 2004) — encodes input structure via Kolmogorov complexity

It was known that the Kolmogorov complexity method was at least as strong as the others, but the precise relationships between all methods were unclear.

## What the paper does

Shows that all five adversary methods above, plus two new formulations (minimax over probability distributions and a generalized spectral adversary SDP), give exactly the same lower bound (up to constant factors for the Kolmogorov variant). There is essentially one quantum adversary method — the different formulations are just different ways of writing the same SDP. The paper also provides a clean proof that all adversary bounds are limited by $\sqrt{C_0 C_1}$ for total functions (where $C_0, C_1$ are certificate complexities), explaining exactly why the adversary method fails for problems like element distinctness and triangle finding.

## The unified SDP framework

The central result is that the following quantities are all equal (Theorem 3.1):

$$\frac{Q_\varepsilon(f)}{1 - 2\sqrt{\varepsilon(1-\varepsilon)}} \geq SA(f) = WA(f) = SWA(f) = MM(f) = SMM(f) = GSA(f) = \Theta(KA(f))$$

### Spectral adversary (SA)

Choose a non-negative symmetric matrix $\Gamma$ with $\Gamma \circ F = \Gamma$ (i.e., $\Gamma[x,y] = 0$ whenever $f(x) = f(y)$). Then:

$$SA(f) = \max_\Gamma \frac{\lambda(\Gamma)}{\max_i \lambda(\Gamma \circ D_i)}$$

where $D_i[x,y] = 1$ iff $x_i \neq y_i$, and $\lambda(\cdot)$ is the spectral norm. This ratio measures: how "connected" are the inputs with different function values (numerator) relative to how much any single variable contributes to distinguishing them (denominator).

### Minimax over probability distributions (MM)

For each input $x$, choose a probability distribution $p_x$ over the $n$ input variables:

$$MM(f) = \frac{1}{\max_p \min_{\substack{x,y \\ f(x) \neq f(y)}} \sum_{i: x_i \neq y_i} \sqrt{p_x(i) \cdot p_y(i)}}$$

This is the dual formulation: instead of constructing an adversary matrix, you try to find distributions that make it *easy* to distinguish different-valued inputs. The adversary lower bound equals the inverse of the best such distinguishing score.

### Generalized spectral adversary (GSA) — an SDP

$$\text{minimize } \text{tr}(\Delta) \quad \text{subject to } \Delta \text{ diagonal}, \; Z \geq 0, \; Z \cdot F = 1, \; \forall i: \Delta - Z \circ D_i \succeq 0$$

This is the SDP dual of the semidefinite version of minimax (SMM).

## Proof structure

The equivalences are established through a circle of reductions:

```
SA ≤ WA  (Thm 4.3, via Mathias's spectral norm bound for Hadamard products)
   ↓
SWA ≤ SA  (Thm 4.4, via a converse decomposition)
   =
WA = SWA = SA
   ↓
MM = SMM  (Thm 5.1, rank-1 Cholesky decomposition shows SDP relaxation is tight)
   =
SMM = GSA  (Thm 5.2, SDP duality via Lagrange multipliers)
   =
GSA = SA  (Thm 6.1, rescaling by diagonal matrix)
   =
KA = Θ(WA)  (Thms 7.1-7.2, from Laplante-Magniez: Shannon-Fano coding connects 
              Kolmogorov complexity to probability distributions)
```

The key technical ingredient for SA $\leftrightarrow$ WA is a lemma on spectral norms of Hadamard products (Lemma 4.1, due to Mathias): if $S \leq M \circ N$ entry-wise, then $\lambda(S) \leq r(M) \cdot c(N)$, where $r(M)$ and $c(N)$ are maximum row and column $\ell_2$-norms. A strengthened version (Lemma 4.2) restricts the max to entries where $S > 0$.

## Certificate complexity barrier

**Theorem 3.2:** For partial functions with output alphabet ordered by certificate complexity $C_0 \geq C_1 \geq \ldots$, the adversary bound satisfies:

$$MM(f) \leq 2\sqrt{C_1 \cdot n}$$

For total Boolean functions, this improves to $MM(f) \leq \sqrt{C_0 \cdot C_1}$.

**Proof (due to de Wolf):** For total $f$, distribute $p_x$ uniformly over a minimal certificate $C_f(x)$. For any $x,y$ with $f(x) \neq f(y)$, the certificates must disagree on some variable $j$, giving:

$$\sum_{i: x_i \neq y_i} \sqrt{p_x(i) \cdot p_y(i)} \geq \sqrt{\frac{1}{|C_f(x)|} \cdot \frac{1}{|C_f(y)|}} \geq \frac{1}{\sqrt{C_0 \cdot C_1}}$$

This barrier explains why the adversary method cannot prove good lower bounds for:
- **Element distinctness** ($C_0 = 2$, so bound $\leq O(\sqrt{n})$ while true complexity is $\Theta(n^{2/3})$)
- **Triangle finding** ($C_0 = 3$)
- **Binary AND-OR trees** ($\sqrt{C_0 C_1} = \sqrt{n}$)

## Key results

| Statement | Bound |
|---|---|
| All adversary methods equivalent | $SA = WA = SWA = MM = SMM = GSA = \Theta(KA)$ |
| Certificate barrier (total Boolean) | Adversary $\leq \sqrt{C_0 \cdot C_1}$ |
| Certificate barrier (partial Boolean) | Adversary $\leq 2\sqrt{C_1 \cdot n}$ |
| Adversary vs query complexity | $Q_\varepsilon(f) \geq (1 - 2\sqrt{\varepsilon(1-\varepsilon)}) \cdot \text{Adv}(f)$ |

## Comparison with prior work

| Method | Formulation style | Proven strength before this paper |
|---|---|---|
| Unweighted adversary (Ambainis 2000) | Combinatorial (0/1 weights) | Weakest; tight for search, 2-level AND-OR |
| Weighted adversary (Ambainis 2003) | Weight scheme $(w, w')$ | Strictly stronger than unweighted |
| Spectral adversary (Barnum-Saks-Szegedy 2003) | Spectral norm of $\Gamma$ matrix | Sufficient conditions for query complexity via SDP |
| Strong weighted (Zhang 2005) | Unified Ambainis variants | $\geq$ weighted |
| Kolmogorov (Laplante-Magniez 2004) | $K(i \mid x, \sigma)$ | $\geq$ all above |
| **This paper** | All equivalent | All give the same bound |

## Limits / caveats

- The adversary method (in all its forms) is fundamentally limited by the certificate complexity barrier. For problems like element distinctness, the polynomial method (Aaronson-Shi) gives tight $\Omega(n^{2/3})$ bounds that are beyond the adversary's reach.
- The adversary method fails completely for exact and zero-error quantum complexity — it can only give the bounded-error lower bound.
- The Kolmogorov complexity formulation is equal only up to constants (depending on the universal Turing machine), not exactly.
- This paper treats only the *positive-weight* adversary. The breakthrough came later with the **negative-weight adversary** (Høyer-Lee-Špalek 2007), which removes the non-negativity constraint on $\Gamma$ and breaks through the certificate complexity barrier. The negative-weight adversary is tight for all Boolean functions (Reichardt 2009).

## Reusable ideas

1. [[SDP Duality for Query Complexity Bounds]] — the equivalence SA = GSA shows that primal adversary matrix formulations and dual SDP formulations give the same bound; moving between primal and dual views is a powerful technique for both proving and understanding lower bounds
2. [[Hadamard Product Spectral Norm Bound]] — the Mathias lemma relating $\lambda(M \circ N)$ to row/column norms provides a clean way to convert between spectral and weighted formulations of adversary bounds
3. [[Certificate Complexity Barrier for Adversary Methods]] — the de Wolf construction shows how distributing weight over certificates yields an explicit upper bound on any adversary-type lower bound, and identifies exactly which problems the adversary method cannot handle

## References within this paper

- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett-Bernstein-Brassard-Vazirani (1997)]] — the hybrid method, which is the starting point for all adversary methods
- Ambainis, "Quantum lower bounds by quantum arguments", JCSS 2002 (arXiv:quant-ph/0002066) — the original unweighted adversary method
- Ambainis, "Polynomial degree vs. quantum query complexity", FOCS 2003 (arXiv:quant-ph/0305028) — the weighted adversary
- Barnum-Saks-Szegedy, "Quantum decision trees and semidefinite programming", CCC 2003 — the spectral method and SDP characterisation
- Zhang, "On the power of Ambainis's lower bounds", TCS 2005 — the strong weighted adversary
- Laplante-Magniez, "Lower bounds for randomized and quantum query complexity using Kolmogorov arguments", CCC 2004 — the Kolmogorov complexity method
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — element distinctness algorithm, which the adversary method cannot optimally lower-bound
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes|Ambainis-Childs-Reichardt-Špalek-Zhang (2007)]] — later work by Špalek on formula evaluation

## Cross-links

### Paper notes
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — the hybrid method ancestor
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]] — uses polynomial method, not adversary
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]] — Špalek co-author; adversary-related techniques for formula evaluation
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — the algorithm the adversary cannot optimally lower-bound
- [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes]] — Szegedy co-author; triangle finding is adversary-limited

### Trick cards
- [[SDP Duality for Query Complexity Bounds]]
- [[Hadamard Product Spectral Norm Bound]]
- [[Certificate Complexity Barrier for Adversary Methods]]
- [[Hybrid Argument Lower Bound for Quantum Search]]
