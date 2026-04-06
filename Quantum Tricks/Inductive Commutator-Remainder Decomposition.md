# Inductive Commutator-Remainder Decomposition

> **Source:** Tran, Chu, Su, Childs, Gorshkov, arXiv:1912.11047
> **Tags:** #trick #trotter #product-formulas #error-analysis

## What it does

Decomposes each order of the Trotter step error into a commutator part $[H, S_k]$ (which telescopes across steps) and a remainder $V_k$ that is one order smaller in system size. This structural decomposition is what enables the [[Commutator-to-Integral Telescoping for Trotter Error Cancellation|telescoping trick]] to work at all orders, not just the leading one.

## The trick

Expand the per-step error $\delta = U_{t/r} - U^{(1)}_{t/r} U^{(2)}_{t/r}$ as a series:

$$
\delta = \sum_{k=2}^{\infty} \frac{(-it)^k}{k! r^k} \delta_k.
$$

**Claim (Lemma 1 in the paper):** For all $k \ge 2$, there exist operators $S_k$, $V_k$ such that

$$
\delta_k = [H, S_k] + V_k
$$

with:
- $\|V_k\| = O(e^{k-2} n^{k-2})$
- $\|S_k\| = O(k^2 n^{k-1})$
- $\|[H, S_k]\| = O(k^3 n^{k-1})$

**Construction.** The base case is $k = 2$: $\delta_2 = [H, H_2]$, so $S_2 = H_2$ and $V_2 = 0$. The inductive step uses the recursion

$$
\delta_{k+1} = H_1 \delta_k + \delta_k H_2 - [H^k, H_2]
$$

and the commutator identities $[H, S_k] H = [H, S_k H]$ and $[H^k, H_2] = [H, \sum_{j=0}^{k-1} H^{k-1-j} H_2 H^j]$ to extract the commutator structure at each order.

The crucial point: $V_k$ scales as $n^{k-2}$ while $[H, S_k]$ scales as $n^{k-1}$. The commutator part dominates in norm but cancels via telescoping; the remainder is smaller and contributes only $O(nt^3/r^2)$ to the total error.

## When to reach for it

- Whenever you need to prove that a sum of conjugated operators cancels across time steps and the error has multiple orders in some expansion parameter
- When you want to show that the "bad" (non-cancelling) part of an error is parametrically smaller than the "good" (cancelling) part
- Analysis of product-formula errors beyond leading order on structured (e.g., lattice) Hamiltonians

## Complexity

No computational cost — purely an analytical technique for bounding errors.

## Caveat

- The construction relies on $H = H_1 + H_2$ with mutually commuting terms within each part. Extending to three or more parts (needed for general 2D lattice models) remains open.
- The exponential growth $e^{k-2}$ in the $V_k$ bound means the series converges only when $nt/r$ is less than $1/e$, requiring $r > ent$.

## Related notes

- [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes]]
- [[Commutator-to-Integral Telescoping for Trotter Error Cancellation]]
- [[Trotter Commutator-Scaling Bound]]
- [[Finite Nested-Commutator Expansion]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
