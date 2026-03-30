# Weight Scheme Adversary Method

> **Source:** Ambainis, arXiv:quant-ph/0002066 (Theorem 6); generalised in arXiv:quant-ph/0305028
> **Tags:** #trick #query-complexity #lower-bound #adversary-method

## What it does

Strengthens the [[Adversary Method for Quantum Query Lower Bounds|basic adversary method]] by allowing non-uniform parameters across input pairs, giving tighter bounds when the "bottleneck" pairs have asymmetric structure.

## The trick

**Extended bound (Theorem 6 of Ambainis 2000):** Instead of global bounds $l, l'$ on the number of partners differing at each index, use per-pair quantities:
- $l_{x,i}$ = number of partners $y$ of $x$ with $x_i \neq y_i$
- $l_{y,i}$ = number of partners $x$ of $y$ with $x_i \neq y_i$
- $l_{\max} = \max_{(x,y) \in R,\, x_i \neq y_i} l_{x,i} \cdot l_{y,i}$

Then $Q_2(f) = \Omega(\sqrt{mm'/l_{\max}})$.

This is strictly better than the basic bound whenever $l_{\max} < l \cdot l'$ — which happens when for every pair $(x,y)$ contributing to the maximum, at least one of $l_{x,i}$ or $l_{y,i}$ is small.

**Weighted generalisation ([[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes|Ambainis 2003]]):** Assign arbitrary positive weights $w(x,y)$ to each pair in $R$, and "distribute" each weight between $x$ and $y$ via auxiliary weights $w'(x,y,i)$ and $w'(y,x,i)$ satisfying $w'(x,y,i) \cdot w'(y,x,i) \geq w(x,y)^2$. The maximum load $v_{\max} = \sqrt{v_A \cdot v_B}$ then gives $Q_2(f) = \Omega(1/v_{\max})$.

Špalek and Szegedy (2004) showed this is equivalent to the spectral adversary method of Barnum-Saks-Szegedy.

## When to reach for it

- The basic adversary bound gives a suboptimal result because $l$ or $l'$ is large, but the product $l_{x,i} \cdot l_{y,i}$ is much smaller for the pairs that actually matter.
- Classic example: **inverting a permutation**. Every $x$ has $N/2$ partners $y$ differing at a given index (so $l_{x,i} = N/2$), but for each such $y$, only $x$ itself is a partner differing there ($l_{y,i} = 1$). So $l_{\max} = N/2$ instead of $l \cdot l' = N/2 \cdot 1$, and the bound is the same — but for the basic method with global $l = N/2$, $l' = N/2$, you'd get a weaker result.
- You need to prove composition theorems for iterated functions (as in [[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes|Ambainis (2003)]]).

## Complexity

Proof technique only.

## Caveat

Still subject to the certificate complexity barrier: $O(\sqrt{C_0(f) \cdot C_1(f)})$ for total functions. The weighted version doesn't escape this. The general adversary bound (negative weights, Høyer-Lee-Špalek 2007) does.

## Related notes
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]
- [[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes]]
- [[Adversary Method for Quantum Query Lower Bounds]]
- [[Hybrid Argument Lower Bound for Quantum Search]]
