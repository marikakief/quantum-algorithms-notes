# Coprime-Factor Splitting for Semidirect-Product HSP

> **Source:** Chi--Kim--Lee, arXiv:quant-ph/0604172
> **Tags:** #trick #hidden-subgroup-problem #group-decomposition #semidirect-product

## What it does

Turns an HSP over a restricted semidirect product into independent HSPs over coprime factors.

## The trick

Suppose a cyclic factor decomposes by the Chinese remainder theorem,

$$
\mathbb{Z}_N\cong \prod_j \mathbb{Z}_{p_j^{r_j}},
$$

and the semidirect action of $\mathbb{Z}_p$ is forced to be trivial on every $q$-power component with $q\neq p$. In Chi--Kim--Lee this follows from $p\nmid q-1$: an order-$p$ automorphism cannot live inside $\mathbb{Z}_{q^s}^*$.

Then

$$
\mathbb{Z}_N\rtimes_\phi \mathbb{Z}_p
\cong
\mathbb{Z}_{N/p^r}\times(\mathbb{Z}_{p^r}\rtimes_\psi\mathbb{Z}_p).
$$

Because the two factors have coprime orders, every subgroup splits:

$$
H\leq G_0\times G_1,\quad \gcd(|G_0|,|G_1|)=1
\quad\Longrightarrow\quad
H=H_0\times H_1.
$$

So the original HSP is just two HSPs glued together: an abelian HSP on $G_0$ and the known semidirect-product HSP on $G_1$.

## When to reach for it

Use this when a nonabelian-looking HSP has a cyclic normal subgroup whose prime-power components are invariant under the action. The test is arithmetic: check whether the acting group has room to act nontrivially on each unit group $\mathbb{Z}_{q^s}^*$.

## Complexity

The splitting itself is group-theoretic bookkeeping. The quantum cost is the sum of the costs of the smaller HSP algorithms, plus efficient CRT/isomorphism computation.

## Caveat

If the action is nontrivial on more than one prime-power component, the coprime-product subgroup split may no longer isolate the hard part. The trick is strongest when the automorphism-order condition kills all but one component.

## Related notes

- [[Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) — Paper Notes]]
- [[Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007) — Paper Notes]]
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]
