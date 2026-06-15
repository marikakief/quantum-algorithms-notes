# Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes

> **Source:** H. F. Chau and H.-K. Lo, *Primality Test Via Quantum Factorization*, arXiv:quant-ph/9508005, Int. J. Mod. Phys. C 8(2):131--138 (1997)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9508005) · [PDF](https://arxiv.org/pdf/quant-ph/9508005)
> **Tags:** #quantum-algorithms #primality-testing #factoring #number-theory #Shor

---

## The computational problem

**Input:** an integer $N$ that has already survived cheap compositeness tests, so the intended use case is proving primality of likely-prime RSA-style candidates.

**Output:** a proof that $N$ is prime, obtained by factoring $N-1$ and checking a Pocklington--Lehmer certificate. If $N$ is composite, the paper's primality-proving procedure may not halt; [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]] is the complementary compositeness/factoring routine.

**Complexity measure:** elementary one- and two-qubit gates, with arithmetic cost based on Schönhage--Strassen multiplication. The stated final bound is

$$
O\!\left((\log N)^3\log\log N\log\log\log N\right)
$$

with $O(\log^2 N)$ extra workspace bits/qubits.

The certificate criterion is the Pocklington--Lehmer $N-1$ test. If

$$
N-1=\prod_{j=1}^m p_j^{\beta_j}
$$

with distinct primes $p_j$, and there exists $a\in\mathbb Z_N$ such that

$$
a^{N-1}\equiv 1\pmod N,
\qquad
 a^{(N-1)/p_j}\not\equiv 1\pmod N\quad\text{for every }j,
$$

then $N$ is prime.

## What the paper does

Chau and Lo turn [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's factoring algorithm]] into a primality prover: factor $N-1$ completely, recursively prove the primality of the factors, then use a Pocklington--Lehmer certificate for $N$. The initial analysis gives

$$
O\!\left((\log N)^3(\log\log N)^2\log\log\log N\right),
$$

and a trial-division front end plus a Brillhart--Lehmer--Selfridge variant removes one $\log\log N$ factor.

My assessment: historically interesting rather than something one would use post-AKS/ECPP. The clean point is certificate structure: unlike [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes|Donis-Vela--Garcia-Escartin]], the final primality certificate is classical once the quantum machine has produced the factorisation chain.

## The algorithm / construction

### 1. Factor $N-1$ completely

Let $M=N-1$. Use [[Order-Finding via QFT and Continued Fractions|Shor order finding]] to obtain a nontrivial factor $f$ of any composite cofactor $M'$ that is not a prime power or twice an odd prime power. Recursively factor $f$ and $M'/f$.

The paper also suggests extracting multiple factors at each split by computing

$$
\gcd(f,M'/f),
$$

which is classically cheap compared with the quantum order-finding call.

Prime powers are handled separately. Classically, one can detect $M=p^n$ in

$$
O\!\left((\log M)^2(\log\log M)^2\log\log\log M\right)
$$

time. Appendix B gives a quantum alternative: for $M=p^n$ with odd prime $p$, the unit group $U(\mathbb Z_M)$ is cyclic of order $p^{n-1}(p-1)$. For random $m\in U(\mathbb Z_M)$, the order $r=\operatorname{ord}_M(m)$ is divisible by $p^{n-1}$ with probability $1-1/p\ge 2/3$, so

$$
\frac{M}{\gcd(M,r)}=p
$$

with probability at least $2/3$.

### 2. Recursively prove the factors are prime

The Pocklington certificate for $N$ needs the $p_j$ in the factorisation of $N-1$ to be prime. The algorithm therefore recursively applies the same primality prover to each $p_j$.

If

$$
N-1=\prod_{i=1}^m p_i^{\beta_i},
$$

then the recursive cost is bounded using

$$
\sum_i \log p_i\le \log N,
\qquad
\sum_i (\log p_i)^3\le (\log N)^3,
$$

and the corresponding monotonicity bounds for the iterated logs.

### 3. Find Pocklington witnesses

The first version samples a random integer $a$ and checks all conditions

$$
a^{N-1}\equiv 1\pmod N,
\qquad
 a^{(N-1)/p_j}\not\equiv 1\pmod N.
$$

There are $O(\log N)$ distinct prime factors $p_j$ in the worst case. Each modular exponentiation is done by repeated squaring, with $O(\log N)$ multiplications; using Schönhage--Strassen multiplication costs

$$
O(\log N\log\log N\log\log\log N)
$$

per multiplication. A single random $a$ therefore costs

$$
O\!\left((\log N)^3\log\log N\log\log\log N\right),
$$

and the paper quotes success probability at least $O(1/\log\log N)$, giving the extra $\log\log N$ factor in the first analysis.

### 4. Remove the extra $\log\log N$ factor

Two modifications give the headline bound.

**Trial division before quantum factoring.** First divide $N-1$ by all integers below a threshold $k$. This costs

$$
O(k\log N\log\log N\log\log\log N).
$$

After this, all remaining prime factors exceed $k$, so there are at most

$$
O\!\left(\frac{\log N}{\log k}\right)
$$

of them. The combined cost is

$$
O\!\left(\log N\log\log N\log\log\log N
\left(k+\frac{(\log N)^2\log\log N}{\log k}\right)\right).
$$

Choosing

$$
k\sim \frac{(\log N)^2}{\log\log N}
$$

gives complete factorisation of $N-1$ in

$$
O\!\left((\log N)^3\log\log N\log\log\log N\right).
$$

If a prime table up to $k$ is available, only $O(k/\log k)$ trial divisions are needed and the optimum shifts to $k\sim \log^2 N$, but the asymptotic order is unchanged.

**Separate witnesses for separate prime factors.** Instead of asking one $a$ to work for all $p_j$, use the Brillhart--Lehmer--Selfridge variant: for each $j$, it suffices to find some $a_j$ with

$$
a_j^{N-1}\equiv 1\pmod N,
\qquad
 a_j^{(N-1)/p_j}\not\equiv 1\pmod N.
$$

For a fixed $p_j$, a random $a_j$ satisfies the second condition with probability at least $1/2$ on prime inputs. A few random samples cover the required witnesses, so the verification stage costs

$$
O\!\left((\log N)^3\log\log N\log\log\log N\right).
$$

## Key results

### Shor-factorisation cost used by the paper

For composite $M$ not of the form $p^n$ or $2p^n$ with odd prime $p$, a nontrivial factor can be found with high probability in

$$
O\!\left((\log M)^2(\log\log M)^2\log\log\log M\right)
$$

elementary qubit operations. Prime powers can be handled within the same asymptotic bound.

### First primality-proving bound

Recursive complete factorisation of $N-1$ plus the original Pocklington--Lehmer witness search yields

$$
P(N)
=O\!\left((\log N)^3(\log\log N)^2\log\log\log N\right).
$$

### Improved primality-proving bound

With trial division below $k\sim (\log N)^2/\log\log N$ and the Brillhart--Lehmer--Selfridge per-prime-witness test,

$$
P(N)
=O\!\left((\log N)^3\log\log N\log\log\log N\right).
$$

The paper also states $O(\log^2 N)$ extra working space.

## Comparison with prior work

| Method | Quantum ingredient | Proves primes? | Treatment of composites | Complexity quoted in/near the paper | Comment |
|---|---|---:|---|---|---|
| APRCL / Jacobi sums | none | yes | deterministic/proving framework | $O((\log N)^c\log\log\log N)$ for some $c>0$ | Practical for sub-1000 digit inputs at the time, not polynomial as stated here. |
| Goldwasser--Kilian / Atkin--Morain ECPP | none | yes | one-sided; may fail on some inputs under implementation assumptions | about $O(\log^6 N)$ in this discussion | Classical certificate route; the paper notes analytic assumptions around practical variants. |
| Miller under ERH | none | yes | deterministic under ERH | $O((\log N)^4\log\log\log N)$ | Conditional classical baseline. |
| Bach--Huelsbergen conjectural test | none | yes | deterministic if conjecture holds | $O((\log N)^3\log\log N\log\log\log N)$ | Matches Chau--Lo asymptotically if the conjecture is true. |
| Chau--Lo | [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]] of $N-1$ | yes | primality prover may not halt on composites | $O((\log N)^3\log\log N\log\log\log N)$ | Quantum work produces a classical Pocklington-style certificate chain. |
| [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes|Donis-Vela--Garcia-Escartin]] | direct order finding on random bases | yes | can output witnesses or remain inconclusive depending on filters | $O((\log n)^2n^3)$ with ordinary multiplication in their notation | Avoids factoring $N-1$, but the positive certificate is quantum-checkable rather than classical. |

## Limits / caveats

- The algorithm is a primality prover, not a clean decision procedure as written. If $N$ is composite, the Pocklington stage need not terminate; one runs [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor factoring]] directly for compositeness.
- The claimed fastest-known status was pre-AKS and should be read historically. Today, the interest is the way quantum factorisation supplies classical certificates, not the raw primality-test headline.
- The asymptotic improvement depends on fast multiplication and trial division thresholds. For realistic sizes, constants and the cost of fault-tolerant modular exponentiation dominate.
- The Pocklington certificate for $N$ depends on recursively proving the primality of every distinct $p_j\mid N-1$. The factorisation output alone is not enough unless the factor certificates are also supplied.
- The paper's notation reuses $m$ both for the number of prime factors and for random bases in places; the intended meaning is clear from context but easy to misread.

## Reusable ideas

1. [[Quantum-Factored Pocklington Certificates]] — use quantum factorisation only to obtain the complete $N-1$ factorisation chain, then finish with a classically checkable Pocklington certificate.
2. [[Trial-Division Front-Loading for Quantum Factorisation]] — remove small factors classically so the number of expensive quantum factor-finding calls drops by a logarithmic factor.
3. [[Per-Prime Pocklington Witness Collection]] — replace one global generator-like witness by separate witnesses for each prime factor of $N-1$.
4. [[Prime-Power Extraction from Order Divisibility]] — in $U(\mathbb Z_{p^n})$, a random order is divisible by $p^{n-1}$ with constant probability, exposing $p$ by a gcd.

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — quantum factoring and order finding; the main subroutine.
- Ekert and Jozsa (1996) — review of Shor's algorithm used as a reference for the order-finding analysis.
- Pocklington--Lehmer $N-1$ test; Riesel (1994) and Cohen (1993) are cited for computational number theory background.
- Brillhart, Lehmer, and Selfridge (1975) — per-prime-witness variant of the Pocklington--Lehmer test.
- Adleman--Pomerance--Rumely (1983), Goldwasser--Kilian (1986), Atkin--Morain (1993), Adleman--Huang (1992) — classical primality-proving baselines.
- Miller (1976) and Bach--Huelsbergen (1993) — conditional/conjectural classical primality-test comparisons.
- Schönhage--Strassen (1971) and Knuth (1981) — multiplication and modular-exponentiation cost model.
- [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes|Donis-Vela--Garcia-Escartin (2017)]] — later direct-order-finding quantum primality test; not cited by Chau--Lo, but the natural later comparison.

## Cross-links

### Paper notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes]]
- [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]]
- [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes]]
- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]]

### Trick cards

- [[Order-Finding via QFT and Continued Fractions]]
- [[Quantum-Factored Pocklington Certificates]]
- [[Trial-Division Front-Loading for Quantum Factorisation]]
- [[Per-Prime Pocklington Witness Collection]]
- [[Prime-Power Extraction from Order Divisibility]]
