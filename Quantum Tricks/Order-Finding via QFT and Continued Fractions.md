
> **Source:** Shor, arXiv:quant-ph/9508027 (1994), §5
> **Tags:** #trick #period-finding #number-theory #continued-fractions

## What it does

Finds the multiplicative order $r$ of $x \bmod N$ (the smallest $r$ with $x^r \equiv 1 \pmod{N}$) using $O(\log\log N)$ quantum experiments, each consisting of one modular exponentiation and one QFT.

## The trick

1. Choose $q = 2^l$ with $N^2 \leq q < 2N^2$
2. Prepare $\frac{1}{\sqrt{q}}\sum_a |a\rangle|x^a \bmod N\rangle$
3. Apply [[Quantum Fourier Transform Circuit|QFT]] to the first register
4. Measure $c$ in the first register
5. Compute the continued fraction expansion of $c/q$
6. Each convergent $p_i/q_i$ with $q_i < N$ is a candidate for $d/r$
7. Check: does $x^{q_i} \equiv 1 \pmod{N}$?

**Why continued fractions work:** The measurement outcome $c$ satisfies $|c/q - d/r| \leq 1/(2q)$ for some integer $d$. Since $q > N^2 > r^2$, there's at most one fraction $d/r$ with $r < N$ in this interval. The continued fraction algorithm finds all best rational approximations to $c/q$ — the correct $d/r$ must appear as a convergent.

**Success per trial:** If $\gcd(d, r) = 1$, we get $r$ directly. This happens with probability $\phi(r)/r = \Omega(1/\log\log r)$. After $O(\log\log N)$ trials, success is overwhelming.

**Practical improvements:** Also try $c \pm 1, c \pm 2, \ldots$. If $d/r$ is obtained but $\gcd(d,r) \neq 1$, we get a divisor of $r$ — combine divisors from multiple trials via LCM.

## When to reach for it

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Factoring]] via order-finding
- Any [[Hidden Subgroup Problem]] on cyclic groups $\mathbb{Z}_N$
- Extracting exact rational eigenvalues from [[Gapped Phase Estimation|phase estimation]] measurements

## Complexity

Per trial: one modular exponentiation ($O(n^2 \log n \log\log n)$ gates) + one QFT ($O(n^2)$ gates) + classical continued fractions ($O(n^2)$ bit operations). Number of trials: $O(\log\log N)$.

## Caveat

Only works when the measured value $c/q$ is close to a fraction with denominator $< N$ — guaranteed by $q > N^2$. If $q$ is too small relative to $r^2$, multiple fractions could satisfy the proximity condition and the method becomes ambiguous.

The continued fraction approach gives $d/r$ in lowest terms. If $\gcd(d,r) > 1$, we only learn a divisor of $r$, not $r$ itself. Multiple trials fix this, but it's the source of the $O(\log\log N)$ overhead.

## Related notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Quantum Fourier Transform Circuit]]
- [[Coset Sampling via Fourier Transform]]
- [[Interference-Based Period Finding]]
- [[Reversible Modular Exponentiation with Garbage Cleanup]]
