# Bounded-Tail Kronecker Expansion via Jacobi-Trudi

> **Source:** Panova, arXiv:2502.20253
> **Tags:** #trick #representation-theory #kronecker-coefficients #littlewood-richardson

## What it does

Computes Kronecker coefficients with one long-row input by reducing them to a bounded alternating sum of multi-Littlewood--Richardson coefficients.

## The trick

Use the identity
$$
s_\nu[xy]=\sum_{\lambda,\mu} g(\lambda,\mu,\nu)s_\lambda(x)s_\mu(y)
$$
and expand $s_\nu$ with Jacobi--Trudi:
$$
s_\nu[xy]=\sum_{\sigma\in S_\ell}\operatorname{sgn}(\sigma)
\prod_i h_{\nu_i+\sigma_i-i}[xy].
$$
Since $h_m=s_{(m)}$ and $g((m),\alpha,\beta)=\mathbf 1[\alpha=\beta]$, coefficient comparison gives
$$
g(\lambda,\mu,\nu)=
\sum_{\sigma\in S_\ell}\operatorname{sgn}(\sigma)
\sum_{\alpha_i\vdash \nu_i+\sigma_i-i}
 c^\lambda_{\alpha_1,\ldots,\alpha_\ell}
 c^\mu_{\alpha_1,\ldots,\alpha_\ell}.
$$
If $\nu_1\ge n-k$, then the total size of $\alpha_2,\ldots,\alpha_\ell$ is at most $k$ after accounting for the permutation shift. The sum is then over bounded-size partitions plus one large partition $\alpha_1$ differing from $\lambda$ or $\mu$ in only $O(k)$ boxes.

## When to reach for it

Use it for Kronecker coefficients where one partition has bounded aft, equivalently polynomial Specht dimension by [[Aft-Bounded Partition Regime for Polynomial Specht Dimension]]. It is also a useful pattern for replacing a hard internal tensor-product multiplicity by ordinary LR data when one factor is close to the trivial representation.

## Complexity

Panova's bounded-aft algorithm runs in
$$
O\!\left(
(\ell(\lambda)\log\lambda_1+\ell(\mu)\log\mu_1)
\min\{\ell(\lambda),\ell(\mu)\}^k k^{k^2+2k}
\right),
$$
where $k=n-\nu_1$. Combined with $f_\nu\le n^c\Rightarrow \operatorname{aft}(\nu)\le 4c^2$, this gives polynomial time for fixed $c$.

## Caveat

The formula is alternating, so it is not a positive combinatorial interpretation. It is an exact computational reduction whose cost is controlled only in the bounded-tail regime.

## Related notes

- [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]]
- [[Aft-Bounded Partition Regime for Polynomial Specht Dimension]]
- [[Kronecker Coefficient as a Diagonal-Invariant Fourier-Label Projector]]
