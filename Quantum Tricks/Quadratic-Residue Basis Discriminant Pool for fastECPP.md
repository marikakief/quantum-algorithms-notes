# Quadratic-Residue Basis Discriminant Pool for fastECPP

> **Source:** Morain, arXiv:math/0502097
> **Tags:** #trick #primality-proving #ECPP #number-theory

## What it does

Turns many expensive modular square-root computations in ECPP into cheap products of precomputed square roots.

## The trick

In ECPP, testing a CM discriminant $-D$ against a candidate prime $N$ requires a square root

$$
\sqrt{-D}\pmod N,
$$

before Cornacchia's algorithm can test

$$
4N=U^2+DV^2.
$$

Instead of computing a fresh square root for every $D$, choose a pool

$$
Q=\{q_1^*,\ldots,q_r^*\}
$$

of small signed prime discriminant factors satisfying

$$
\left(\frac{q_i^*}{N}\right)=1.
$$

Compute each $\sqrt{q_i^*}\bmod N$ once. Then form candidate discriminants from products, for example

$$
-D=q_i^*q_j^*,
$$

and obtain

$$
\sqrt{-D}\equiv \sqrt{q_i^*}\sqrt{q_j^*}\pmod N
$$

by modular multiplication.

With $r=O(\log N)$, the pool gives $O((\log N)^2)$ pair discriminants, which is the same search volume ordinary ECPP uses, but only $O(\log N)$ modular square-root calls are paid.

## When to reach for it

- A search repeatedly needs square roots of many products modulo a fixed modulus.
- Candidate parameters factor into small pieces with individually testable residuosity.
- The square-root operation is much more expensive than multiplying residues.

## Complexity

For $L=\log N$ and multiplication cost $O(L^{1+\mu})$, ordinary ECPP spends $O(L^2L^{2+\mu})$ on $O(L^2)$ square roots per level. The basis method spends

$$
O(LL^{2+\mu})
$$

on square roots plus cheaper combination and Cornacchia work. In Morain's fastECPP analysis, this lowers the discriminant phase from $O(L^{4+\mu})$ to $O(L^{3+\mu})$ per recursion level.

## Caveat

The pool changes the distribution of discriminants. Products of signed prime factors have genus-theory structure, and Morain notes that composite discriminants of this kind have even class number. The paper treats the resulting bias as harmless in practice, not as a theorem.

## Related notes

- [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes]]
- [[Delayed Multiprecision Cornacchia Screening]]
- [[Smooth Class-Number Filtering for CM Polynomial Splitting]]
