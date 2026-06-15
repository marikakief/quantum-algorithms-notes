# Lucas Certificate by Direct Quantum Order Finding

> **Source:** Donis-Vela and Garcia-Escartin, arXiv:1711.02616
> **Tags:** #trick #primality-testing #order-finding #number-theory

## What it does

Certifies primality by finding a unit modulo $N$ whose multiplicative order is exactly $N-1$.

## The trick

Use the Lucas converse to Fermat's theorem in order form. For any $a\in\mathbb Z_N^*$,

$$
\operatorname{ord}_N(a)\mid \varphi(N).
$$

If exact [[Order-Finding via QFT and Continued Fractions|quantum order finding]] returns

$$
\operatorname{ord}_N(a)=N-1,
$$

then $N$ must be prime, because a composite $N>1$ has $\varphi(N)<N-1$.

For prime $N$, the group $\mathbb Z_N^*$ is cyclic of size $N-1$, and exactly $\varphi(N-1)$ bases have order $N-1$. A random base therefore succeeds with probability

$$
\frac{\varphi(N-1)}{N-1}
>\frac{1}{3\log\log(N-1)}
$$

for large enough $N$.

The certificate is just the base $a$. It is short, but checking it requires exact order finding unless the verifier also receives classical information such as a factorisation of $N-1$.

## When to reach for it

- You already have a quantum order-finding routine and want primality certification without first factoring $N-1$.
- A quantum-checkable certificate is acceptable.
- The input is being preprocessed before [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]], where ruling out prime inputs is useful.

## Complexity

Donis-Vela and Garcia-Escartin quote one order-finding call as

$$
O((\log n)n^3)
$$

operations for an $n$-bit $N$ using ordinary multiplication. The expected number of candidate bases for prime $N$ is $O(\log\log N)=O(\log n)$, so the primality test costs

$$
O((\log n)^2n^3).
$$

With asymptotic fast multiplication, the quoted full cost is

$$
O(\log\log n(\log n)^3n^2).
$$

## Caveat

This swaps a classical certificate for a quantum one. If the verifier is classical, the base $a$ alone is not enough to check primality efficiently in general. This is the main difference from Lucas--Lehmer/Pratt-style certificates and Chau--Lo's quantum-factoring route.

## Related notes

- [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Euler-Liar Screening Before Quantum Order Finding]]
- [[Primitive-Root Prime-Power Detection by Order GCD]]
