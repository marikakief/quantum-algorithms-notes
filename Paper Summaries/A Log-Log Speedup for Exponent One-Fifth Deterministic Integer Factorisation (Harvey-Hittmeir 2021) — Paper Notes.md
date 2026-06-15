# A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) — Paper Notes

> **Source:** David Harvey and Markus Hittmeir, *A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation*, arXiv:2105.11105 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2105.11105) · [PDF](https://arxiv.org/pdf/2105.11105)
> **Tags:** #paper #classical-number-theory #factoring #deterministic-algorithms

---

## The computational problem

Given an integer $N \ge 2$, compute its prime factorisation on a deterministic multi-tape Turing machine. The paper writes $F(N)$ for the bit complexity.

The main theorem is stated for full integer factorisation, but the proof reduces to the case where $N$ is prime or semiprime. If $N$ has at least three prime factors, one factor is at most $N^{1/3}$ and can be found in $N^{1/6+o(1)}$ time, below the final $N^{1/5}$ bound. The hard case is therefore

$$
N=pq,\qquad p<q,
$$

or deciding that $N$ is prime.

This is classical factoring, not a quantum algorithm. It belongs in the Zoo batch because it is part of the algebraic/number-theoretic landscape around [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor-style factoring]], but the result here is about rigorous deterministic classical time.

## What the paper does

Harvey and Hittmeir shave a $(\log\log N)^{3/5}$ factor from Harvey's previous exponent-$1/5$ deterministic factoring algorithm:

$$
F(N)=O\!\left(\frac{N^{1/5}\log^{16/5}N}{(\log\log N)^{3/5}}\right).
$$

My assessment: technically neat, but incremental in exponent. The exponent stays $1/5$, but the paper is a rigorous asymptotic improvement in the deterministic factoring frontier: the new content is a small-prime coprimality sieve folded into the Lehman/Harvey collision framework without breaking the proof.

## The algorithm / construction

The algorithm refines the deterministic Lehman-style search used in Hittmeir and Harvey's earlier work.

### 1. Reduce to prime-or-semiprime input

For composite $N$ with at least three prime factors, first find a factor at most $N^{1/3}$ in $N^{1/6+o(1)}$ time using the deterministic small-factor search tools in the Pollard-Strassen/Coppersmith-Harvey line, not naive trial division. This cost is negligible relative to the theorem. The rest of the algorithm assumes $N$ is either prime or $N=pq$ with distinct primes $p<q$.

### 2. Force a large-order modular element

Run Hittmeir's large-order subroutine with

$$
D=\lceil N^{2/5}\rceil.
$$

It returns one of three outcomes:

1. a non-trivial factor of $N$;
2. a proof that $N$ is prime;
3. an element $\beta\in \mathbb Z_N^*$ with $\operatorname{ord}_N(\beta)>D$.

The cost is

$$
O\!\left(\frac{D^{1/2}\lg^2 N}{(\lg\lg D)^{1/2}}\right)
=O\!\left(\frac{N^{1/5}\lg^2 N}{(\lg\lg N)^{1/2}}\right),
$$

which is below the main search cost.

This large-order element is later used only through $\beta^{m^2}$. Since $\operatorname{ord}(\beta^{m^2})=\operatorname{ord}(\beta)/\gcd(\operatorname{ord}(\beta),m^2)$, the parameter choice must leave it above the collision-search threshold `2 lambda + 1`. With the displayed choices, $D/m^2$ is still asymptotically larger than the required $\lambda$; common-factor cases with the primorial are handled by the gcd checks and the proof of Proposition 4.7.

### 3. Sieve candidate residues of $p$ by small primes

Let

$$
B=\left\lceil \frac{\lg N}{30}\right\rceil,
\qquad
m=\prod_{2\le r\le B\atop r\text{ prime}} r,
$$

and set

$$
m_0=\left\lceil
\frac{N^{1/5}(\lg\lg N)^{2/5}}{m(\lg N)^{4/5}}
\right\rceil.
$$

If $\gcd(N,m)$ is non-trivial, we are done. Otherwise $p$ and $q$ are coprime to $m$, so $p$ lies in one of only $\varphi(m)$ residue classes modulo $m$, not all $m$ classes. Mertens' theorem gives

$$
\frac{\varphi(m)}{m}=O\!\left(\frac{1}{\lg\lg N}\right),
$$

which is exactly where the log-log saving enters.

### 4. Compute constrained Lehman coefficients by a rank-2 lattice reduction

For each coprime residue $\sigma \bmod m$ and each geometric interval

$$
\sigma_0\le p < \left(1+\frac{1}{m_0}\right)\sigma_0,
$$

Proposition 3.3 computes integers $(a,b)\ne(0,0)$ such that, if $p\equiv \sigma\pmod m$, then

$$
\left|aq+bp-\left(a\frac{N}{\sigma_0}+b\sigma_0\right)\right|
\le
\frac{4N^{1/2}m^{1/2}}{m_0^{3/2}},
$$

and also

$$
aq+bp\equiv a\frac{N}{\sigma}+b\sigma \pmod {m^2}.
$$

The congruence is enforced by solving

$$
-a\frac{N}{\sigma}+b\sigma\equiv 0\pmod m,
$$

which is equivalent to $b\equiv \gamma a\pmod m$ for $\gamma\equiv N/\sigma^2\pmod m$. The solutions form the lattice

$$
L=\langle (1,\gamma),(0,m)\rangle\subseteq \mathbb Z^2.
$$

A linear change of variables turns the desired bounds into a square, and a two-dimensional Lagrange--Gauss reduction finds a short nonzero vector in $O(\lg^3 N)$ bit operations. This is the paper's cleanest technical device.

### 5. Babystep/giantstep collision search modulo an unknown prime factor

Define

$$
\lambda=\left\lceil \frac{4N^{1/2}}{(m m_0)^{3/2}}\right\rceil.
$$

The babysteps are

$$
\beta^{m^2 i}\pmod N,
\qquad i=0,\ldots,2\lambda.
$$

For each pair $(\sigma,\sigma_0)$, the algorithm computes a giantstep

$$
v_{\sigma,\sigma_0}=\beta^{aN+b-j_{\sigma,\sigma_0}}\pmod N,
$$

where $j_{\sigma,\sigma_0}$ is chosen from the approximation to $aq+bp$ and the residue modulo $m^2$. For the one pair matching the true residue class and interval of $p$, there is an $i$ with

$$
\beta^{m^2 i}\equiv v_{\sigma,\sigma_0}\pmod p.
$$

If the equality holds modulo $N$, a direct check using the candidate

$$
u=m^2 i+j_{\sigma,\sigma_0}
$$

tests whether $u=aq+bp$ and recovers $p,q$ from the quadratic

$$
y^2-u y+abN.
$$

If the collision exists only modulo $p$ or only modulo $q$, the algorithm uses the Harvey collision subroutine: build

$$
f(x)=\prod_h (x-v_h)\in \mathbb Z_N[x]
$$

with a product tree, evaluate $f(1),f(\alpha),\ldots,f(\alpha^{\kappa-1})$ for $\alpha=\beta^{m^2}$ using Bluestein's algorithm, then take GCDs with $N$ to reveal a non-trivial factor.

## Key results

### Theorem 1.1

There is a deterministic integer factorisation algorithm with

$$
F(N)=O\!\left(\frac{N^{1/5}\log^{16/5}N}{(\log\log N)^{3/5}}\right)
$$

bit operations.

### Proposition 3.3: constrained Lehman coefficients

Given $N\ge 2$, positive integers $m_0,\sigma_0,m$ with $m$ coprime to $N$, and a residue $\sigma$ coprime to $m$, the algorithm outputs $(a,b)\ne(0,0)$ such that for any semiprime $N=pq$ with

$$
\sigma_0\le p<\left(1+\frac{1}{m_0}\right)\sigma_0,
\qquad p\equiv \sigma\pmod m,
$$

one has

$$
\left|aq+bp-\left(a\frac{N}{\sigma_0}+b\sigma_0\right)\right|
\le
\frac{4N^{1/2}m^{1/2}}{m_0^{3/2}}
$$

and

$$
aq+bp\equiv a\frac{N}{\sigma}+b\sigma\pmod {m^2}.
$$

For $m_0,\sigma_0,m=O(N)$, the coefficient algorithm costs $O(\lg^3 N)$ bit operations and outputs coefficients with polynomially many bits; the proof notes the sharper bounds $|a|=O(N^{1/2})$ and $|b|=O(N^{3/2})$ under the displayed inequalities.

### Proposition 4.4: main search complexity

Given $m,m_0=O(N)$, $\gcd(m,N)=1$, and $\beta\in\mathbb Z_N^*$ such that

$$
\operatorname{ord}_N(\beta^{m^2})\ge 2\lambda+1,
\qquad
\lambda=\left\lceil \frac{4N^{1/2}}{(m m_0)^{3/2}}\right\rceil,
$$

Algorithm 4.3 factors semiprime $N$ or returns that $N$ is prime in time

$$
O\!\left(
 m\lg N(\lg\lg N)^2
 +\varphi(m)m_0\lg^4 N
 +\frac{N^{1/2}\lg^2 N}{(m m_0)^{3/2}}
\right).
$$

### Proposition 4.7: parameter choice

With

$$
m\asymp \prod_{r\le \lg N/30}r,
\qquad
m m_0\asymp \frac{N^{1/5}(\lg\lg N)^{2/5}}{(\lg N)^{4/5}},
$$

the second and third terms of Proposition 4.4 balance, and Mertens' estimate for $\varphi(m)/m$ yields the theorem's $(\log\log N)^{-3/5}$ factor.

## Comparison with prior work

| Work | Model | Bound / role | What changes here |
|---|---:|---:|---|
| Pollard--Strassen--Coppersmith line | deterministic classical | $N^{1/4+o(1)}$ | baseline before Hittmeir |
| Hittmeir 2020 | deterministic classical | $N^{2/9+o(1)}$ | first break below exponent $1/4$ |
| Harvey 2020 | deterministic classical | $O(N^{1/5}\log^{16/5}N)$ | same exponent; no small-prime residue saving |
| Harvey--Hittmeir 2021 | deterministic classical | $O(N^{1/5}\log^{16/5}N/(\log\log N)^{3/5})$ | restricts $p$ to $\varphi(m)$ coprime residue classes modulo a primorial $m$ |
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor 1994]] | quantum | polynomial in $\log N$ | unrelated model; cited only as a faster non-classical option |

## Limits / caveats

- The improvement is only a log-log factor. It does not change the $N^{1/5}$ exponent.
- The algorithm is asymptotic. Proposition 4.7 assumes $N\ge N_0$ for an unspecified constant $N_0$ large enough for the inequalities to hold.
- The method is rigorous and deterministic, but not a practical factoring route against cryptographic moduli. Quantum order finding remains the qualitatively different story.
- The log-log saving depends on knowing that the hidden prime factor is coprime to a primorial $m$. If $\gcd(N,m)$ is non-trivial the problem is already solved; otherwise the sieve reduces the giantstep count from $m$ to $\varphi(m)$.
- The collision-search subroutine, not the rank-2 lattice reduction, is the bottleneck in the displayed bound.

## Reusable ideas

1. [[Primorial Coprimality Sieve for Deterministic Factoring]] — restrict a hidden prime factor to reduced residue classes modulo a small primorial, gaining a Mertens-theorem $1/\log\log N$ density factor.
2. [[Lattice-Constrained Lehman Linear Combinations]] — compute small coefficients $(a,b)$ satisfying both an approximation condition for $aq+bp$ and a congruence condition modulo $m$.
3. [[Product-Tree Bluestein Collision Search Modulo an Unknown Factor]] — detect a collision modulo an unknown prime divisor of $N$ by evaluating a product polynomial on a geometric progression and taking GCDs.

## References within this paper

- Hittmeir 2020 — introduces the time-space tradeoff for Lehman's deterministic factorisation method and obtains $N^{2/9+o(1)}$.
- Harvey 2020 — exponent-$1/5$ deterministic factorisation algorithm; this paper is a direct log-log refinement.
- Hittmeir 2018 — large-order element subroutine used as Lemma 2.5.
- Lehman 1974 — original near-square linear-combination idea for deterministic factoring.
- Bostan--Gaudry--Schost 2007 and Costa--Harvey 2014 — earlier deterministic factoring algorithms using fast recurrence/polynomial ideas.
- Harvey--van der Hoeven 2021 — integer multiplication in $O(n\log n)$, used in the arithmetic cost model.
- Bluestein 1970 — fast evaluation on geometric progressions used in the collision subroutine.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor 1994]] — cited only as part of the faster quantum factoring landscape.

## Cross-links

### Paper notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]]
- [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes]]
- [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes]]

### Trick cards

- [[Primorial Coprimality Sieve for Deterministic Factoring]]
- [[Lattice-Constrained Lehman Linear Combinations]]
- [[Product-Tree Bluestein Collision Search Modulo an Unknown Factor]]
