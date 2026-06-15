# Nonabelian Orthogonal Sampling via a Custom Pairing

> **Source:** Roetteler--Beth, arXiv:quant-ph/9812070
> **Tags:** #trick #hidden-subgroup-problem #nonabelian-fourier-transform #wreath-product

## What it does

Makes nonabelian Fourier sampling look like abelian orthogonal-complement sampling by choosing a pairing adapted to the group.

## The trick

For $W_n=\mathbb{Z}_2^n\wr\mathbb{Z}_2$, define a bijection

$$
\varphi(x,y;0)=(x,y,0),
\qquad
\varphi(x,y;1)=(y,x,1)
$$

and a pairing

$$
\mu(g,h)=\langle\varphi(g),\varphi(h)\rangle_{\mathbb{F}_2^{2n+1}}.
$$

The Fourier transform is ordered so that

$$
\operatorname{DFT}_{W_n}[g,h]=(-1)^{\mu(g,h)}.
$$

Then for a subgroup $U$,

$$
\operatorname{DFT}_{W_n}\frac{1}{\sqrt{|U|}}\sum_{u\in U}|u\rangle
=
\frac{1}{\sqrt{|U^\perp|}}\sum_{v\in U^\perp}|v\rangle,
$$

where

$$
U^\perp=\{v:\mu(v,u)=0\text{ for all }u\in U\}.
$$

Coset states give either $U^\perp$ or $(U^t)^\perp$, depending on whether the random coset representative lies in the base group.

## When to reach for it

Use it when a nonabelian Fourier transform can be arranged to expose a bilinear-looking pairing. This can turn representation theory into linear algebra.

## Complexity

For $W_n$, the transform uses $2n$ Hadamards plus simple controlled permutations, so the circuit size is linear in $n$ up to the elementary-gate implementation of the controls.

## Caveat

$U^\perp$ need not be a subgroup for arbitrary $U$. The algorithm samples both $U^\perp$ and $(U^t)^\perp$ and then works with the group they generate.

## Related notes

- [[Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998) — Paper Notes]]
- [[Balanced-Subgroup Factorization for Wreath-Product HSP]]
- [[Basis-Selected Strong Fourier Sampling for Affine HSP]]
