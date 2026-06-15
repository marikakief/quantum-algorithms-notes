# Modulo-p Slope Extraction for Semidirect-Product HSP

> **Source:** Cosme--Portugal, arXiv:quant-ph/0703223; related to the line test used in [[Fourier Line Test for Classified Semidirect-Product HSP]]
> **Tags:** #trick #hidden-subgroup-problem #fourier-sampling #semidirect-product

## What it does

Recovers a hidden slope parameter $t$ after subgroup classification narrows a semidirect-product HSP to one tilted generator.

## The trick

Suppose the only remaining choices are an untilted subgroup and a tilted subgroup such as

$$
\langle x^p\rangle
\quad\text{or}\quad
\langle x^t y^p\rangle,
\qquad t\in\mathbb{Z}_p^*.
$$

Prepare a two-register superposition over a rectangle of exponents and query the oracle:

$$
\frac{1}{\sqrt{p^3}}\sum_{a'=0}^{p-1}\sum_{b'=0}^{p^2-1}|a'\rangle|b'\rangle|f(x^{a'}y^{b'})\rangle.
$$

If the hidden subgroup is tilted, measuring the oracle register leaves a line state

$$
\frac{1}{\sqrt p}\sum_{\ell=0}^{p-1}|a_0+t\ell\bmod p\rangle|b_0+p\ell\rangle.
$$

Apply $F_p\otimes F_{p^2}$. The measured pair $(a,b)$ satisfies

$$
at+b\equiv0\pmod p.
$$

When $a\not\equiv0\pmod p$, compute

$$
t=-a^{-1}b\pmod p,
$$

then verify the candidate directly by querying whether $f(x^t y^p)=f(1)$.

## When to reach for it

Use it after group theory has already reduced the HSP to a single unknown additive slope over $\mathbb{Z}_p$ or $\mathbb{Z}_{p^2}$. It is not meant to discover the subgroup shape from scratch.

## Complexity

One run succeeds with probability $1-1/p$ in the tilted case. Repetition gives high confidence. The arithmetic and Fourier transforms have cost polynomial in $\log p$ and the exponent register sizes.

## Caveat

The test depends on a prior classification theorem. Without knowing that the candidates are exactly a small list of tilted subgroups, the measured equation $at+b=0$ has no unique interpretation.

## Related notes

- [[Quantum Algorithm for the Hidden Subgroup Problem on a Class of Semidirect Product Groups (Cosme-Portugal 2007) — Paper Notes]]
- [[Fourier Line Test for Classified Semidirect-Product HSP]]
- [[Notes on the Hidden Subgroup Problem on Some Semi-Direct Product Groups (Chi-Kim-Lee 2006) — Paper Notes]]
