# Quantum-Factored Pocklington Certificates

> **Source:** Chau and Lo, arXiv:quant-ph/9508005
> **Tags:** #trick #primality-testing #factoring #number-theory #certificate

## What it does

Turns quantum factorisation into a classically checkable primality certificate by using it only to build the complete $N-1$ factorisation chain.

## The trick

For an integer $N$, first factor

$$
N-1=\prod_{j=1}^m p_j^{\beta_j}
$$

using [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]], and recursively certify that every $p_j$ is prime. Then find Pocklington--Lehmer witnesses. The one-witness form asks for an $a\in\mathbb Z_N$ such that

$$
a^{N-1}\equiv 1\pmod N,
\qquad
 a^{(N-1)/p_j}\not\equiv 1\pmod N\quad\forall j.
$$

Given the factorisation of $N-1$ and the witness data, a classical verifier checks the certificate by modular exponentiation. The quantum computer is not part of certificate verification; it is only the fast way to produce the otherwise hard factorisation data.

## When to reach for it

- You want a quantum-assisted primality prover whose final certificate can be checked classically.
- You have access to fast order finding/factoring, but you do not want the positive certificate itself to require a quantum verifier.
- You are comparing certificate tradeoffs with direct order-finding tests such as [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes]].

## Complexity

In [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]], recursive factorisation of $N-1$ plus the original Pocklington witness search gives

$$
O\!\left((\log N)^3(\log\log N)^2\log\log\log N\right),
$$

then trial division and per-prime witnesses reduce this to

$$
O\!\left((\log N)^3\log\log N\log\log\log N\right).
$$

Classical verification of the final certificate is polynomial and uses the same modular-exponentiation checks.

## Caveat

This is a primality-proving wrapper, not a general compositeness decision algorithm. On composite inputs the Pocklington search may fail to terminate; one should run [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]] directly if the goal is to split a composite.

## Related notes

- [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]]
- [[Per-Prime Pocklington Witness Collection]]
- [[Trial-Division Front-Loading for Quantum Factorisation]]
- [[Order-Finding via QFT and Continued Fractions]]
