# Euler-Liar Screening Before Quantum Order Finding

> **Source:** Donis-Vela and Garcia-Escartin, arXiv:1711.02616
> **Tags:** #trick #primality-testing #order-finding #modular-arithmetic

## What it does

Filters random bases with cheap classical modular exponentiation before spending a quantum order-finding call.

## The trick

Before running exact order finding on a base $a\in\mathbb Z_N^*$, test whether it even has a chance to certify primality.

The light screen used in Donis-Vela--Garcia-Escartin is

$$
b=a^{(N-1)/2}\bmod N.
$$

Then:

- if $b\not\equiv \pm 1\pmod N$, $a$ is a compositeness witness;
- if $b\equiv 1\pmod N$, then $\operatorname{ord}_N(a)\mid (N-1)/2$, so $a$ cannot have order $N-1$ and should be discarded;
- if $b\equiv -1\pmod N$, then $a^{N-1}\equiv 1\pmod N$, so $\operatorname{ord}_N(a)\mid N-1$ and an order-finding call is worth trying.

A stronger variant is Miller--Rabin preselection. Write

$$
N-1=2^s d,
$$

with $d$ odd. Keep only bases satisfying

$$
a^d\equiv 1\pmod N
\quad\text{or}\quad
\exists r<s: a^{2^r d}\equiv -1\pmod N.
$$

For composite $N$, at most one quarter of bases are strong liars, so $k$ clean Miller--Rabin trials leave error at most $4^{-k}$. Bases with $a^d\equiv 1$ can still be discarded before quantum order finding because their order divides $d$.

## When to reach for it

- Quantum order finding is the expensive step and classical modular exponentiation is relatively cheap.
- Many candidate bases are expected to have order too small to be useful.
- You want compositeness witnesses as a byproduct of candidate selection.

## Complexity

Each screen costs one or a few modular exponentiations plus a gcd. In the paper's accounting, this is lower order compared with a quantum order-finding call costing

$$
O((\log n)n^3)
$$

with ordinary multiplication, but it can reduce the number of quantum calls in practice.

## Caveat

Passing the screen does not imply primality and does not even imply that $a$ has order $N-1$. Carmichael numbers and strong liars can pass classical screens; exact order finding is still needed for a positive certificate.

## Related notes

- [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes]]
- [[Lucas Certificate by Direct Quantum Order Finding]]
- [[Order-Finding via QFT and Continued Fractions]]
