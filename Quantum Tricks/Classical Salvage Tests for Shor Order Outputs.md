# Classical Salvage Tests for Shor Order Outputs

> **Source:** Grosshans-Lawson-Morain-Smith, arXiv:1511.04385
> **Tags:** #trick #factoring #post-processing #number-theory

## What it does

Uses cheap classical tests to extract factors from an order-finding output before paying for another QOFA run.

## The trick

In [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]], a run is often treated as failed if the recovered order $r$ is odd or if

$$
a^{r/2}\equiv -1\pmod N.
$$

Grosshans--Lawson--Morain--Smith point out that $r$ can still contain useful arithmetic information. Before rerunning QOFA, try:

1. **Shared factor with $N$.** Compute

   $$
   \gcd(r,N).
   $$

   If this is neither $1$ nor $N$, it is a nontrivial factor. This is common when $N$ has square factors, but it never helps for RSA moduli.

2. **Small-prime divisor tests.** If a small prime $\ell$ divides $r$, compute

   $$
   \gcd(a^{r/\ell}-1,N).
   $$

   More generally, test whether $a^{r/\ell}$ is nontrivial modulo some but not all prime-power factors of $N$. This is the same logic as Shor's even-order test, but with $\ell\in\{2,3,5,\ldots\}$ rather than only $2$.

3. **Recovering $\lambda(N)$ from near-complete orders.** If evidence suggests

   $$
   r=2^{-k}\lambda(N),
   $$

   then try reconstructing $\lambda(N)$ and factor from it, using Miller-style classical post-processing.

## When to reach for it

- After a QOFA call gives an order or order divisor that the standard Shor wrapper would reject.
- In factoring experiments where QOFA calls dominate the cost.
- In structured inputs where $\lambda(N)$ has a simple enough factorisation that divisors are informative.

## Complexity

Negligible next to QOFA: GCDs, modular exponentiation with known classical exponents, and trial division of the recovered $r$ by small primes. In the paper's simulation over composite $N\in[10,10^8]$ with $a=2$, these tests reduced the proportion escaping factorisation after one correct QOFA call from about $6\%$ to $1.5\%$.

## Caveat

This is opportunistic post-processing, not a general theorem matching the safe-semiprime result. For RSA moduli, $\gcd(r,N)$ gives nothing; small-prime divisor tests may also fail, and a fresh QOFA call may still be needed.

## Related notes

- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Safe-Semiprime Factoring from a QOFA Divisor]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
