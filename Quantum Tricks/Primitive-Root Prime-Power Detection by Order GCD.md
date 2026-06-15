# Primitive-Root Prime-Power Detection by Order GCD

> **Source:** Donis-Vela and Garcia-Escartin, arXiv:1711.02616
> **Tags:** #trick #number-theory #order-finding #prime-powers

## What it does

Detects prime powers by finding a primitive root modulo $N$ and taking a gcd between its order and $N$.

## The trick

Primitive roots modulo $N$ exist only for

$$
N\in\{2,4,p^k,2p^k\},
$$

where $p$ is an odd prime. If $N=p^k$, then

$$
\varphi(N)=p^{k-1}(p-1).
$$

Suppose quantum order finding gives a base $a\in\mathbb Z_N^*$ with maximal order

$$
\operatorname{ord}_N(a)=\varphi(N).
$$

Then

$$
\gcd(\operatorname{ord}_N(a),N)=p^{k-1}.
$$

This exposes the prime base via

$$
p=\frac{N}{\gcd(\operatorname{ord}_N(a),N)}.
$$

One then checks $N=p^k$ by repeated division. The same idea handles the $2p^k$ family with a parity adjustment.

## When to reach for it

- You are preprocessing inputs before [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]], where prime powers should be handled outside the main factoring routine.
- You already have exact order finding available.
- You want a quantum route to prime-power detection rather than relying only on classical perfect-power algorithms.

## Complexity

For $N=p^k$, the probability of sampling a primitive root is

$$
\frac{\varphi(\varphi(N))}{\varphi(N)}.
$$

The same totient-ratio estimate used for prime inputs gives $O(\log\log \varphi(N))$ order-finding attempts with high probability. The final repeated-division check is polynomial in the bitlength of $N$.

## Caveat

This is a side observation in the paper, not its main primality test. Classical perfect-power detection is already fast, so the trick is mainly useful when order finding is already part of the pipeline.

## Related notes

- [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes]]
- [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]]
- [[Prime-Power Extraction from Order Divisibility]]
- [[Lucas Certificate by Direct Quantum Order Finding]]
- [[Order-Finding via QFT and Continued Fractions]]
