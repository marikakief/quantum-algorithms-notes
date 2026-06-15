# Trial-Division Front-Loading for Quantum Factorisation

> **Source:** Chau and Lo, arXiv:quant-ph/9508005
> **Tags:** #trick #factoring #number-theory #hybrid-algorithms

## What it does

Removes small factors classically before quantum factor finding, reducing the number of expensive Shor calls on the remaining cofactor.

## The trick

Suppose a quantum algorithm must completely factor $M=N-1$. Before using [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]], perform trial division by all integers below a threshold $k$.

This costs

$$
O(k\log N\log\log N\log\log\log N)
$$

using the arithmetic model in [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]]. After small factors are stripped, every remaining prime factor is $>k$, so the number of distinct remaining prime factors is bounded by

$$
O\!\left(\frac{\log N}{\log k}\right).
$$

The quantum part therefore costs fewer recursive factor-finding calls. Chau--Lo balance the two terms as

$$
O\!\left(\log N\log\log N\log\log\log N
\left(k+\frac{(\log N)^2\log\log N}{\log k}\right)\right),
$$

choosing

$$
k\sim \frac{(\log N)^2}{\log\log N}.
$$

## When to reach for it

- The quantum subroutine is expensive and only needed for nontrivial large cofactors.
- The goal is complete factorisation, so many small factors would otherwise cause many recursive quantum calls.
- You can tolerate cheap classical preprocessing before coherent computation.

## Complexity

With the threshold above, the complete factorisation of $N-1$ in Chau--Lo costs

$$
O\!\left((\log N)^3\log\log N\log\log\log N\right)
$$

rather than carrying an extra $\log\log N$ factor from repeatedly encountering small prime divisors. A prime table improves constants and shifts the balancing point to $k\sim\log^2 N$, but not the asymptotic order.

## Caveat

This is an asymptotic hybrid trick. In real fault-tolerant cost models, the best threshold depends on modular-arithmetic constants, memory, and how many Shor calls can be batched or reused.

## Related notes

- [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]]
- [[Quantum-Factored Pocklington Certificates]]
- [[Order-Finding via QFT and Continued Fractions]]
