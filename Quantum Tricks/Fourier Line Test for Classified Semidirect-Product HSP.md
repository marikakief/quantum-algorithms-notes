# Fourier Line Test for Classified Semidirect-Product HSP

> **Source:** Chi--Kim--Lee, arXiv:quant-ph/0604172; based on Inui--Le Gall's semidirect-product HSP algorithm
> **Tags:** #trick #hidden-subgroup-problem #fourier-sampling #semidirect-product

## What it does

Recovers a hidden slope parameter $h\in\mathbb{Z}_p$ once subgroup classification reduces the HSP to cosets of lines in $\mathbb{Z}_p^2$.

## The trick

For classified subgroups of $\mathbb{Z}_{2p^r}\rtimes\mathbb{Z}_p$, after finding

$$
H\cap\langle x\rangle=\langle x^{2^t p^s}\rangle,
$$

the remaining possibilities are either the cyclic subgroup above or

$$
H=\langle x^{2^t p^s}, x^{h2^t p^{s-1}}y\rangle.
$$

Prepare

$$
\frac{1}{p}\sum_{a,b\in\mathbb{Z}_p}|a\rangle|b\rangle|f(x^{a2^t p^{s-1}}y^b)\rangle.
$$

If the noncyclic case holds, measuring the oracle register leaves a line state

$$
\frac{1}{\sqrt p}\sum_{b\in\mathbb{Z}_p}|a_0+bh\rangle|b\rangle.
$$

Applying $F_p\otimes F_p$ gives support only on pairs $(c,d)$ satisfying

$$
ch+d=0\pmod p.
$$

Whenever $c\neq 0$, output

$$
h=-c^{-1}d\pmod p,
$$

implemented as $h=-c^{p-2}d\pmod p$.

## When to reach for it

Use it after a subgroup-structure lemma has reduced a nonabelian HSP to one unknown additive slope over $\mathbb{Z}_p$. It is basically Fourier sampling on an affine line, but wrapped inside semidirect-product notation.

## Complexity

One run succeeds with probability $1-1/p$ in the noncyclic case. Constantly many repetitions distinguish it from the cyclic case with high probability. Chi--Kim--Lee quote total time $O((r\log p)^2)$ for the $\mathbb{Z}_{2p^r}\rtimes\mathbb{Z}_p$ appendix instance after the abelian-HSP step.

## Caveat

The test is not a general nonabelian-HSP tool. It needs the prior classification that says every candidate subgroup is controlled by a single $h\in\mathbb{Z}_p$.

## Related notes

- [[Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) — Paper Notes]]
- [[Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
