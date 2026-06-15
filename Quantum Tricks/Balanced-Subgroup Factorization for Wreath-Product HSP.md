# Balanced-Subgroup Factorization for Wreath-Product HSP

> **Source:** Roetteler--Beth, arXiv:quant-ph/9812070
> **Tags:** #trick #hidden-subgroup-problem #wreath-product #group-decomposition

## What it does

Splits the hidden subgroup in $W_n=\mathbb{Z}_2^n\wr\mathbb{Z}_2$ into an abelian base part and a balanced part.

## The trick

Let

$$
W_n=(\mathbb{Z}_2^n\times\mathbb{Z}_2^n)\rtimes\mathbb{Z}_2,
\qquad
N=\mathbb{Z}_2^n\times\mathbb{Z}_2^n,
$$

and let $t=(0,0;1)$ be the element that swaps the two base factors. For every subgroup $U\leq W_n$,

$$
U=(U\cap N)(U\cap U^t).
$$

The first factor is inside the abelian normal subgroup $N$, so it is found by the abelian HSP. The second factor detects whether $U$ has elements outside $N$ and is reconstructed by nonabelian Fourier sampling.

## When to reach for it

Use this when a semidirect product has an index-2 abelian normal subgroup and conjugation by the outside element has a simple action. The subgroup can often be reconstructed by pairing the base intersection with a conjugation-invariant component.

## Complexity

The decomposition itself is group theory. In Roetteler--Beth, the base part is learned with the abelian HSP over $\mathbb{Z}_2^{2n}$ and the balanced part is learned with $O(n)$ samples plus linear algebra over $\mathbb{F}_2$.

## Caveat

The equality relies on the special order-four behavior of $W_n$ and the swap action. It should not be assumed for arbitrary index-2 semidirect products.

## Related notes

- [[Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998) — Paper Notes]]
- [[Nonabelian Orthogonal Sampling via a Custom Pairing]]
- [[Base-Group Restriction for Wreath-Product HSP]]
