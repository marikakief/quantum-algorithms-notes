# Order-Finding via QFT and Continued Fractions

> **Source:** Shor, arXiv:quant-ph/9508027 (1994), §5
> **Tags:** #trick #period-finding #number-theory #continued-fractions

## What it does

Finds the multiplicative order $r$ of $x \bmod N$ (the smallest $r$ with $x^r \equiv 1 \pmod{N}$). Here $N$ is the modulus and $n=\lceil \log_2 N\rceil$ is the bit length. Shor's direct success analysis uses $O(\log\log r)=O(\log n)$ quantum experiments for high probability, each consisting of one modular exponentiation and one QFT; additional classical post-processing can reduce expected quantum trials in many cases.

## The trick

1. Choose $q = 2^l$ with $N^2 \leq q < 2N^2$
2. Prepare $\frac{1}{\sqrt{q}}\sum_a |a\rangle|x^a \bmod N\rangle$
3. Apply [[Quantum Fourier Transform Circuit|QFT]] to the first register
4. Measure $c$ in the first register
5. Compute the continued fraction expansion of $c/q$
6. Each convergent $p_i/q_i$ with $q_i < N$ is a candidate for $d/r$
7. Check: does $x^{q_i} \equiv 1 \pmod{N}$?

**Why continued fractions work:** A good measurement outcome $c$ satisfies $|c/q - d/r| \leq 1/(2q)$ for some integer $d$. This is not guaranteed for every outcome, but Shor lower-bounds the total probability of useful outcomes. Since $q > N^2 > r^2$, there is at most one fraction $d/r$ with $r < N$ in this interval. The continued fraction algorithm finds all best rational approximations to $c/q$ — for a good outcome, the correct reduced fraction appears as a convergent.

**Success per experiment:** If the sample is good and $\gcd(d, r) = 1$, we get $r$ directly. Shor's bound gives success probability at least $\phi(r)/(3r)=\Omega(1/\log\log r)$. After $O(\log\log r)=O(\log n)$ direct repetitions, success is overwhelming.

**Practical improvements:** Also try $c \pm 1, c \pm 2, \ldots$, test small multiples of convergent denominators, and combine candidate orders from multiple trials via LCM. Every candidate $r'$ must be validated by checking $x^{r'} \equiv 1 \pmod N$.

## When to reach for it

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Factoring]] via order-finding
- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes|Safe-semiprime factoring]] where a single order divisor at base $2$ is enough with probability $1-1/(q_1q_2)$
- Any [[Hidden Subgroup Problem]] on cyclic groups $\mathbb{Z}_N$
- Extracting exact rational eigenvalues from [[Gapped Phase Estimation|phase estimation]] measurements

## Complexity

Per experiment: one modular exponentiation plus one QFT plus classical continued fractions. With schoolbook reversible arithmetic, modular exponentiation is $O(n^3)$ gates. In Shor's fast-multiplication asymptotic accounting, it is $O(n^2 \log n \log\log n)$ gates. The QFT is $O(n^2)$ gates exactly, and classical continued fractions/candidate checks are polynomial in $n$. Keep this per-experiment cost separate from the $O(\log\log r)=O(\log n)$ direct repetition overhead.

## Caveat

The continued-fraction extraction only works on measured values $c/q$ close enough to some $d/r$ with denominator $r<N$. The condition $q>N^2$ guarantees uniqueness of such a fraction when the good-approximation event occurs; it does not make every measurement outcome good. If $q$ is too small relative to $r^2$, multiple fractions could satisfy the proximity condition and the method becomes ambiguous.

The continued fraction approach gives $d/r$ in lowest terms. If $\gcd(d,r) > 1$, we only learn a divisor of $r$, not $r$ itself. Multiple trials fix this, but it's the source of the $O(\log\log N)$ overhead.

## Related notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes]]
- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]]
- [[Quantum Fourier Transform Circuit]]
- [[Coset Sampling via Fourier Transform]]
- [[Interference-Based Period Finding]]
- [[Reversible Modular Exponentiation with Garbage Cleanup]]
