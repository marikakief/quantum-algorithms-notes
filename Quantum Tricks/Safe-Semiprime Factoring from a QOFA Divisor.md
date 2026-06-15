# Safe-Semiprime Factoring from a QOFA Divisor

> **Source:** Grosshans-Lawson-Morain-Smith, arXiv:1511.04385
> **Tags:** #trick #factoring #order-finding #number-theory

## What it does

Turns a single divisor of $\operatorname{ord}_N(2)$ into the factors of a safe semiprime $N=(2q_1+1)(2q_2+1)$.

## The trick

For a safe semiprime

$$
N=p_1p_2=(2q_1+1)(2q_2+1),
$$

we have

$$
\phi(N)=4q_1q_2,\qquad \lambda(N)=2q_1q_2.
$$

If QOFA is run at the fixed base $2$, then the order is forced into

$$
\operatorname{ord}_N(2)\in\{q_1q_2,2q_1q_2\}.
$$

The QOFA output is a divisor

$$
d=\frac{\operatorname{ord}_N(2)}{\gcd(t,\operatorname{ord}_N(2))}.
$$

Convert it to an even number

$$
s=\begin{cases}
d, & 2\mid d,\\
2d, & 2\nmid d.
\end{cases}
$$

Except when $s=2$, the value of $s$ identifies enough of the hidden factorisation:

- If $s<N/3$, then $s=2q_i$ and $p_i=s+1$.
- If $s>N/3$, then $s=2q_1q_2$ and $\phi(N)=2s$. The factors are the roots of

  $$
  X^2-(N-2s+1)X+N=0.
  $$

Equivalently, set $t=(N+1)/2-s$ and output

$$
t\pm\sqrt{t^2-N}.
$$

## When to reach for it

- Special-form RSA-like moduli where $p_i-1$ has known sparse factorisation shape.
- Post-processing order-finding outputs when you expect $\lambda(N)$ to have very few divisors.
- Analysing whether a “failed” [[Order-Finding via QFT and Continued Fractions|order-finding]] sample still carries enough arithmetic information to recover factors.

## Complexity

One QOFA call plus polynomial-time integer arithmetic: parity test, comparison with $N/3$, one square root, and exact division. For the safe-semiprime setting, success probability is

$$
1-\frac{1}{q_1q_2}\sim1-\frac{4}{N}.
$$

## Caveat

The trick relies on the safe-semiprime divisor lattice. Once $\lambda(N)$ has more prime factors, a divisor of $\operatorname{ord}_N(a)$ may no longer identify whether it corresponds to $q_1$, $q_2$, some other prime factor of $p_i-1$, or their product.

## Related notes

- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Classical Salvage Tests for Shor Order Outputs]]
