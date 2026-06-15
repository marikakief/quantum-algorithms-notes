# Prime-Power Extraction from Order Divisibility

> **Source:** Chau and Lo, arXiv:quant-ph/9508005
> **Tags:** #trick #order-finding #prime-powers #factoring #number-theory

## What it does

Factors an odd prime power $M=p^n$ by using order finding to expose the $p^{n-1}$ part of the unit-group order.

## The trick

For odd prime $p$, the unit group

$$
U(\mathbb Z_{p^n})
$$

is cyclic of order

$$
p^{n-1}(p-1).
$$

Pick a random $m\in U(\mathbb Z_{p^n})$ and use [[Order-Finding via QFT and Continued Fractions|quantum order finding]] to compute

$$
r=\operatorname{ord}_{p^n}(m).
$$

In a cyclic group of order $p^{n-1}(p-1)$, the probability that $r$ is divisible by $p^{n-1}$ is

$$
1-\frac{1}{p}\ge \frac{2}{3}.
$$

When this happens,

$$
\gcd(M,r)=p^{n-1},
$$

so the prime base is exposed by

$$
p=\frac{M}{\gcd(M,r)}.
$$

After $p$ is known, repeated division checks the exponent $n$.

## When to reach for it

- A Shor-style factoring routine has a special case for prime powers.
- Classical perfect-power detection is unavailable or you want a quantum-only side routine using the same order-finding machinery.
- You need to peel off $p$ from $p^n$ rather than merely recognise that a prime power is present.

## Complexity

One order-finding call costs the usual Shor-order-finding amount. Chau--Lo quote

$$
O\!\left((\log M)^2(\log\log M)^2\log\log\log M\right)
$$

for finding an order with high enough success probability, and the order-divisibility event itself has constant probability at least $2/3$.

## Caveat

Classical prime-power detection is already cheap compared with general factoring. This trick matters mainly inside a uniform order-finding/factoring analysis, not as a practical improvement by itself.

## Related notes

- [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]]
- [[Primitive-Root Prime-Power Detection by Order GCD]]
- [[Order-Finding via QFT and Continued Fractions]]
