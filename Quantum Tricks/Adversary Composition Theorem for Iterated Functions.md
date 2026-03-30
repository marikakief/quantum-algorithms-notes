# Adversary Composition Theorem for Iterated Functions

> **Source:** Ambainis, arXiv:quant-ph/0305028
> **Tags:** #trick #query-complexity #lower-bound #adversary-method #composition

## What it does

Shows that the [[Weight Scheme Adversary Method|weighted adversary]] lower bound composes multiplicatively under function iteration: if the base function $g$ has adversary lower bound $Q$, then the $d$-fold iteration $g^d$ has lower bound $Q^d$.

## The trick

Let $g : \{0,1\}^n \to \{0,1\}$ have a weight scheme with maximum load $v_1$, giving $Q_2(g) = \Omega(1/v_1)$.

Define $g^d$ by iterating: $g^{d+1}(x) = g(g^d(\mathbf{x}_1), \ldots, g^d(\mathbf{x}_n))$.

**Theorem:** $g^d$ has a weight scheme with maximum load $v_1^d$, so $Q_2(g^d) = \Omega(1/v_1^d) = \Omega(Q_2(g)^d)$.

**Construction:** For the iterated function, build the weight scheme inductively:
- The relation $R_d$ relates inputs $(x,y)$ that differ in one "block" according to $R_{d-1}$ (for blocks where the base function changes value) and agree elsewhere.
- Pair weights: $w_d(x,y) = w_1(\tilde{x}, \tilde{y}) \cdot \prod_j (\text{sub-block contribution})$
- Per-index weights inherit from both levels via the factored structure.

The total weight factorises as $\text{wt}_d(x) = \text{wt}_1(\tilde{x}) \cdot \prod_j \text{wt}_{d-1}(x^j)$, and the load factorises as $v_d \leq v_1 \cdot v_{d-1}$. By induction, $v_d \leq v_1^d$.

## When to reach for it

- You want a quantum lower bound for an iterated/composed function and already have a weight scheme for the base function.
- You're proving a separation between polynomial degree and quantum query complexity: if $\deg(g) = d_0$ and $1/v_1 > d_0$, then for the $k$-fold iteration, $Q_2(g^k) = \Omega((1/v_1)^k)$ while $\deg(g^k) = d_0^k$, giving a superlinear gap.
- More broadly, any time you need lower bounds on composed functions $f \circ g^{\otimes n}$ in the query model.

## Complexity

Proof technique only.

## Caveat

- Only applies when the weight scheme has the right multiplicative structure. Not all adversary lower bounds compose this cleanly — the weight scheme must be constructible in the factored form.
- Subject to the same [[Weight Scheme Adversary Method|certificate complexity barrier]] as the weighted adversary method itself.
- Doesn't compose with the [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes|polynomial method]] — the composition is specific to the adversary framework.

## Related notes
- [[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes]]
- [[Weight Scheme Adversary Method]]
- [[Adversary Method for Quantum Query Lower Bounds]]
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]
