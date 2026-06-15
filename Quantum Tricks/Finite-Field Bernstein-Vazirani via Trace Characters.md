# Finite-Field Bernstein-Vazirani via Trace Characters

> **Source:** Montanaro, arXiv:1105.3310; de Beaudrap-Cleve-Watrous, quant-ph/0011065; van Dam-Hallgren-Ip, quant-ph/0211140
> **Tags:** #trick #Bernstein-Vazirani #finite-fields #QFT #phase-kickback

## What it does

Learns a linear form over $\mathbb{F}_q^n$ with one quantum query, even with an unknown additive constant.

## The trick

Let

$$
g(x)=a\cdot x+\beta
$$

where $a\in\mathbb{F}_q^n$ and $\beta\in\mathbb{F}_q$. Use the finite-field character

$$
\chi(z)=\omega^{\operatorname{Tr}(z)},\qquad \omega=e^{2\pi i/p},
$$

for $q=p^r$, with trace $\operatorname{Tr}:\mathbb{F}_q\to\mathbb{F}_p$.

One oracle query plus phase kickback prepares

$$
\frac{1}{q^{n/2}}\sum_{x\in\mathbb{F}_q^n}\omega^{\operatorname{Tr}(a\cdot x+\beta)}|x\rangle.
$$

Apply the inverse QFT over $\mathbb{F}_q^n$. Orthogonality of characters sends the state to

$$
\omega^{\operatorname{Tr}(\beta)}|a\rangle.
$$

The constant $\beta$ is only a global phase, so measuring gives $a$ exactly.

## When to reach for it

- Generalising [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani]] beyond $\mathbb{F}_2$.
- Learning affine linear forms over finite fields.
- Hidden shift / finite-field Fourier algorithms where oracle values live in $\mathbb{F}_q$.
- Polynomial-learning reductions that expose linear coefficient vectors.

## Complexity

One query to $g$, plus QFTs over $\mathbb{F}_q$ on $n$ registers and finite-field arithmetic.

## Caveat

The query model must allow addition of $g(x)$ into a field-valued response register. Gate costs depend on how $\mathbb{F}_q$ is represented and how the QFT over $\mathbb{F}_q$ is compiled.

## Related notes

- [[Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012) — Paper Notes]]
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]]
- [[Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) — Paper Notes]]
- [[Finite-Difference Linearisation for Polynomial Learning]]
