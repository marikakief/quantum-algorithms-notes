# Per-Prime Pocklington Witness Collection

> **Source:** Chau and Lo, arXiv:quant-ph/9508005; Brillhart--Lehmer--Selfridge variant
> **Tags:** #trick #primality-testing #number-theory #certificate

## What it does

Speeds up Pocklington-style certificate search by allowing a different witness for each prime divisor of $N-1$.

## The trick

The strict Pocklington--Lehmer form asks for one $a$ such that, for every distinct prime $p_j\mid N-1$,

$$
a^{N-1}\equiv 1\pmod N,
\qquad
 a^{(N-1)/p_j}\not\equiv 1\pmod N.
$$

A useful variant only requires separate witnesses $a_j$:

$$
a_j^{N-1}\equiv 1\pmod N,
\qquad
 a_j^{(N-1)/p_j}\not\equiv 1\pmod N
\quad\text{for each }j.
$$

This still proves primality once the $p_j$ are known to be prime. For prime $N$, a random base satisfies the nontriviality condition for a fixed $p_j$ with probability at least $1/2$, so a small number of random bases supplies witnesses for all prime factors with high probability.

Conceptually, this relaxes “find a generator-like element” into “separately rule out each maximal subgroup”.

## When to reach for it

- You have the complete factorisation of $N-1$ and need a fast certificate search.
- One global witness is too restrictive or has an annoying $1/\log\log N$ success probability.
- A certificate can store a short list of witnesses rather than one base.

## Complexity

In [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]], the per-prime-witness variant reduces the Pocklington verification stage to

$$
O\!\left((\log N)^3\log\log N\log\log\log N\right),
$$

matching the trial-division-improved factorisation stage.

## Caveat

The factorisation of $N-1$ must be complete, and the primality of each $p_j$ must already be certified. Without that, the Pocklington implication is not valid.

## Related notes

- [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]]
- [[Quantum-Factored Pocklington Certificates]]
- [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]]
