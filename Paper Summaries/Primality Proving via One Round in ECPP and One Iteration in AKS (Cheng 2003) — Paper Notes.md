# Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes

> **Source:** Qi Cheng, *Primality Proving via One Round in ECPP and One Iteration in AKS*, arXiv:math/0301179, Journal of Cryptology 20, 375–387 (2007)
> **Links:** [arXiv](https://arxiv.org/abs/math/0301179) · [PDF](https://arxiv.org/pdf/math/0301179)
> **Tags:** #primality-proving #number-theory #AKS #ECPP #classical-algorithms

---

## The computational problem

**Input:** a positive integer $n$.

**Output:** either a compositeness witness, or a primality certificate for $n$ that a deterministic verifier can check without assuming unproved number-theory conjectures.

**Cost measure:** bit complexity in $\tilde O(\cdot)$ notation, with `log` meaning base-$2$ and `ln` meaning natural logarithm. Cheng assumes fast integer/polynomial multiplication for the quoted fastest bounds.

The paper is not a quantum-algorithm paper. It belongs in the Zoo batch because it sits in the algebraic/number-theoretic algorithm chain around primality proving. The method is classical: combine one ECPP-style elliptic-curve reduction with one AKS/Berrizbeitia-style polynomial congruence check.

## What the paper does

Cheng gives a random primality-proving algorithm with heuristic expected time

$$
\tilde O(\log^4 n),
$$

certificate length $O(\log n)$, and deterministic verification time $\tilde O(\log^4 n)$.

The technical move is sensible: do not run full ECPP down to a tiny prime, and do not run the full AKS family of congruences. Instead, use one ECPP reduction to land on a nearby prime $n'$ whose $n'-1$ has a medium-sized prime divisor $r \in [\log^2 n',2\log^2 n']$, then certify $n'$ by one polynomial congruence modulo $x^r-a$.

My assessment: the idea is neat but heavily heuristic. The advertised quartic time rests on two distribution claims: ECPP-style curve orders behave randomly in the Hasse interval, and “good” primes have enough density in short intervals. Neither is proved here.

## The algorithm / construction

### Good primes

Cheng calls $n$ **$C$-good** if $n-1$ has a prime factor $p$ satisfying

$$
\log^2 n \le p \le C\log^2 n.
$$

The paper then focuses on **good** meaning $2$-good. For such an $n$, let $r$ be a prime divisor of $n-1$ with

$$
\log^2 n \le r \le 2\log^2 n,
$$

and let $r^\alpha\Vert n-1$. This $r$ is a prime factor of $n-1$ used by Cheng's certificate; it is not the unrelated order parameter called $r$ in the original AKS algorithm.

### One-congruence certificate for a good prime

For a candidate good prime $n$:

1. Reject if $n$ is a perfect power.
2. Run a compositeness-proving test in parallel, e.g. Rabin--Miller. If it finds a witness, output `composite`.
3. Pick a random $1<b<n$.
4. If $b^{n-1}\not\equiv 1 \pmod n$, output `composite`.
5. Set

   $$
   a \equiv b^{(n-1)/r^\alpha} \pmod n.
   $$

   If $a=1$ or $a^{r^{\alpha-1}}=1$, try a new $b$. These conditions are not just random-base conveniences: they force the relevant $r$-power order needed for the later irreducibility argument.
6. If

   $$
   \gcd(a^{r^{\alpha-1}}-1,n)\ne 1,
   $$

   output `composite`.
7. Check the polynomial congruence

   $$
   (1+x)^n \equiv 1+x^n \pmod{n, x^r-a}.
   $$

   If it fails, output `composite`; if it passes, the data $(r,\alpha,a)$ certify the reduced prime candidate.

The cost of the polynomial step is $\tilde O(r\log^2 n)=\tilde O(\log^4 n)$ because $r=O(\log^2 n)$.

### One ECPP round to reach a good prime

For a general probable prime $n$, if $n-1$ is not already good, Cheng runs one ECPP-style reduction:

1. Find an elliptic curve $E/\mathbb Z_n$ whose number of points has the form $\omega n'$.
2. Here $\omega$ is fully factored, and $n'$ is a probable prime satisfying the ECPP size condition

   $$
   n' > (\sqrt[4]{n}+1)^2.
   $$

3. Cheng additionally asks that $n'$ be good and lie in the Hasse interval

   $$
   n-2\sqrt n+1 \le n' \le n+2\sqrt n+1.
   $$

4. Produce a point $P\in E(\mathbb Z_n)$ of order $n'$ and use the standard ECPP proposition: if the large cofactor condition is met and the relevant denominators are coprime to $n$, primality of $n'$ implies primality of $n$.
5. Certify $n'$ by the one-congruence AKS-style test above.

The final certificate contains the ECPP curve/point/order data plus $a$ for the polynomial congruence.

ASCII view:

```text
candidate n
   |
   | if n-1 has medium prime factor r: skip ECPP
   v
one ECPP round
   |
   | produces nearby probable prime n' with n'-1 divisible by r ~ log^2 n'
   v
good n'
   |
   | one polynomial congruence modulo (n', x^r-a)
   v
certificate for n' + ECPP lift back to n
```

## Key results

### Main theorem: one polynomial congruence certifies primality

Let $n$ not be a power of an integer. Suppose there is a prime $r$ with $r^\alpha\Vert n-1$, $\alpha\ge 1$, and

$$
r\ge \log^2 n.
$$

Suppose there is $1<a<n$ such that

$$
a^{r^\alpha}\equiv 1\pmod n,
$$

$$
\gcd(a^{r^{\alpha-1}}-1,n)=1,
$$

and

$$
(1+x)^n \equiv 1+x^n \pmod{n,x^r-a}.
$$

Then $n$ is prime.

The proof generalises Berrizbeitia's $x^{2^s}-a$ variant. The key field-theoretic ingredients are:

- some prime divisor $p\mid n$ has $r^\alpha\Vert p-1$ and $a$ is not an $r$-th power in $\mathbb F_p$;
- hence $x^r-a$ is irreducible over $\mathbb F_p$;
- in $\mathbb F_p(\theta)$ with $\theta^r=a$, the congruence forces a large subgroup $G_n$ with $|G_n|\ge 2^r$;
- pigeonholing automorphisms $\sigma_{d^i p^j}$ for $0\le i,j\le \lfloor\sqrt r\rfloor$ gives two identical automorphisms, so $|G_n|$ divides $d^{i_1}p^{j_1}-d^{i_2}p^{j_2}$;
- the bound $d^{i_1}p^{j_1}-d^{i_2}p^{j_2}<n^{\lfloor\sqrt r\rfloor}\le 2^{\sqrt r\log n}\le 2^r$ forces equality, so $n$ is a power of $p$; since perfect powers were excluded, $n=p$.

### Density support for good numbers

Let

$$
m=\prod_{b_1\le p\le c b_1}p.
$$

Numbers in $[1,m]$ with a prime factor in $[b_1,cb_1]$ are exactly the zero-divisors of $\mathbb Z/m\mathbb Z$. Using Mertens' estimate,

$$
\prod_{p<x}\left(1-\frac1p\right)=\frac{e^{-\gamma}}{\ln x}\left(1+O\left(\frac1{\ln x}\right)\right),
$$

Cheng shows that for a suitable absolute constant $c$ and large $b_1$,

$$
1-\prod_{b_1\le p\le c b_1}\left(1-\frac1p\right)>\frac1{\ln b_1}.
$$

This is evidence for the good-prime heuristic, not a theorem about primes in short intervals.

### Heuristic runtime and certificate size

Assuming:

1. ECPP heuristics for finding suitable elliptic-curve orders in the Hasse interval;
2. Cheng's short-interval good-prime density conjecture;
3. fast multiplication;

then the expected running time is

$$
\tilde O(\log^4 n).
$$

The certificate has $O(\log n)$ bits, and deterministic verification takes

$$
\tilde O(\log^4 n).
$$

## Comparison with prior work

| Method | Prover time | Verification time | Certificate size | Main bottleneck |
|---|---:|---:|---:|---|
| AKS original | proved polynomial; heuristic $\tilde O(\log^6 n)$ in this paper's discussion | roughly same as proving | none in the certificate sense | many polynomial congruences modulo $x^r-1$ |
| Berrizbeitia variant | $\tilde O(\log^4 n)$ for primes with a large power of two in $p-1$ | $\tilde O(\log^4 n)$ | small witness $a$ | low density of easily proved primes, about $1/\log^2 p$ heuristically |
| ECPP | heuristic $\tilde O(\log^6 n)$, or $\tilde O(\log^5 n)$ with fast multiplication, as quoted here | $\tilde O(\log^3 n)$ with fast multiplication | longer recursive certificate | many recursive curve-order reductions |
| Cheng | heuristic $\tilde O(\log^4 n)$ | $\tilde O(\log^4 n)$ | $O(\log n)$ | unproved density of good primes in short intervals; polynomial-space burden |

The tradeoff is clear: Cheng spends more verification time than ECPP but tries to save prover time and certificate size by stopping ECPP after one useful reduction.

Rabin--Miller appears in the randomized prover as a compositeness-witness finder. It is not an assumption in the deterministic certificate verifier once the ECPP data and the AKS-style congruence witness exist.

## Limits / caveats

- The running time is heuristic. The hardest missing piece is a short-interval statement: among primes in $[n-2\sqrt n+1,n+2\sqrt n+1]$, a $\Omega(1/\ln\log^2 n)$ fraction should be $2$-good.
- The ECPP step also uses the usual heuristic that orders of small-discriminant CM curves behave like random integers in the Hasse interval.
- The paper's density theorem counts integers with medium prime factors, not primes in short intervals.
- The polynomial congruence is memory-heavy. Cheng notes that for a $1000$-bit $n$, the intermediate polynomial in $(1+x)^n\bmod(n,x^r-a)$ can be around $2^{30}$ bits, about 128 MB in the paper's estimate.
- This is classical, not quantum. It is useful background for number-theoretic algorithm comparisons, but it does not give a quantum speedup.

## Reusable ideas

1. [[Medium-Prime-Factor Targeting by One ECPP Round]] — use one elliptic-curve primality-proving reduction not to shrink all the way down, but to land in an arithmetically convenient subfamily.
2. [[Single-Congruence AKS Certificate over a Binomial Modulus]] — replace many AKS congruence checks by one check modulo $x^r-a$ when $r\mid n-1$ and $a$ has controlled $r$-power order.
3. [[Zero-Divisor Density Heuristic for Medium Prime Factors]] — estimate how often integers have a prime factor in $[b,cb]$ by counting zero-divisors in a primorial modulus.

## References within this paper

- Agrawal, Kayal, and Saxena (2002) — original deterministic polynomial-time primality test; cited for the $x^r-1$ congruence framework.
- Berrizbeitia (2002) — AKS sharpening using $x^{2^s}-a$; Cheng's main theorem generalises this direction.
- Goldwasser--Kilian (1986), Atkin (1986), and Atkin--Morain (1993) — ECPP background and the elliptic-curve reduction proposition.
- Bach--Shallit (1996) — algorithmic number theory background, including Rabin--Miller.
- Lenstra--Lenstra (1990), Morain (1998), and [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes|Morain (2005)]] — ECPP complexity and implementation context.
- Tenenbaum (1995) — Mertens-product estimate used for the medium-prime-factor density calculation.

## Cross-links

### Paper notes

- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]] — another number-theoretic primality/factoring-adjacent note in this batch; quantum rather than classical.
- [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes]] — fastECPP implementation background for the ECPP reduction machinery used here.

### Trick cards

- [[Medium-Prime-Factor Targeting by One ECPP Round]]
- [[Single-Congruence AKS Certificate over a Binomial Modulus]]
- [[Zero-Divisor Density Heuristic for Medium Prime Factors]]
