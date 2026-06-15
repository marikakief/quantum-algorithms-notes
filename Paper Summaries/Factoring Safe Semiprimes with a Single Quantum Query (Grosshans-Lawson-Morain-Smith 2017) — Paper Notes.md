# Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes

> **Source:** Frédéric Grosshans, Thomas Lawson, François Morain, and Benjamin Smith, *Factoring Safe Semiprimes with a Single Quantum Query*, arXiv:1511.04385, v3 (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1511.04385) · [PDF](https://arxiv.org/pdf/1511.04385)
> **Tags:** #factoring #Shor #order-finding #number-theory #cryptography

---

## The computational problem

**Input.** An odd composite integer

$$
N=p_1p_2=(2q_1+1)(2q_2+1),
$$

where $p_1,p_2$ are distinct safe primes and $q_1,q_2>2$ are distinct primes. The algorithm also handles the exceptional case $5\mid N$ separately.

**Output.** The complete factorisation $\{p_1,p_2\}$, or `Failure` in the rare case where the order-finding sample is uninformative.

**Oracle/subroutine model.** One call to the same quantum order-finding subroutine used in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's factoring algorithm]]. On input $(a,N)$, QOFA returns

$$
d=\frac{\operatorname{ord}_N(a)}{\gcd(t,\operatorname{ord}_N(a))}
$$

for uniformly random $t\in\{1,\ldots,\operatorname{ord}_N(a)\}$, idealising away finite-QFT rounding. This is the usual continued-fraction output of [[Order-Finding via QFT and Continued Fractions]].

**Cost measure.** Number of QOFA calls is the main measure. Classical post-processing is polynomial time in $\log N$ and tiny compared with modular exponentiation/order finding.

---

## What the paper does

It shows that safe semiprimes are unusually friendly inputs for quantum factoring. For $N=(2q_1+1)(2q_2+1)$, a single QOFA call at the fixed base $a=2$ gives enough information to factor $N$ with probability

$$
1-\frac{1}{q_1q_2}\sim 1-\frac{4}{N}.
$$

The improvement is entirely in the classical wrapper around Shor's order-finding routine. My assessment: it is not a new quantum primitive, but it is a neat example of squeezing more from a costly quantum sample by using the structure of $\lambda(N)$ rather than blindly applying the standard $\gcd(a^{r/2}\pm1,N)$ test. The fixed base $2$ is valuable because the same modular-exponentiation circuit can be reused; it is not a base chosen with knowledge of the factors.

---

## The algorithm / construction

The number-theoretic setup is:

$$
\phi(N)=4q_1q_2,\qquad \lambda(N)=\operatorname{lcm}(p_1-1,p_2-1)=2q_1q_2.
$$

For any $a\in(\mathbb Z/N\mathbb Z)^\times$, $\operatorname{ord}_N(a)\mid\lambda(N)$, so possible orders divide $2q_1q_2$. The key specialisation is to take $a=2$.

### Algorithm 1: FactorSafeSemiprime

Given safe semiprime $N=p_1p_2$ with $p_i=2q_i+1$:

1. If $5\mid N$, return $\{5,N/5\}$.
2. Run QOFA once on $(2,N)$ and obtain $d$.
3. Convert the possibly odd divisor into an even candidate:

   $$
   s=\begin{cases}
   d, & 2\mid d,\\
   2d, & 2\nmid d.
   \end{cases}
   $$

   For successful QOFA outputs, $s\in\{2q_1,2q_2,2q_1q_2\}$.
4. If $s=2$, return `Failure`.
5. If $s<N/3$, then $s=2q_i$ for one of the two primes. Return

   $$
   p=s+1,\qquad N/p.
   $$

6. Otherwise $s=2q_1q_2$. Then $\phi(N)=2s$. For a semiprime, knowing $\phi(N)$ gives the factors as roots of

   $$
   X^2-(N-\phi(N)+1)X+N=0.
   $$

   Substituting $\phi(N)=2s$, set

   $$
   t=\frac{N+1}{2}-s.
   $$

   The two factors are

   $$
   t\pm\sqrt{t^2-N}.
   $$

The branch test $s<N/3$ works because for safe semiprimes with $q_i>2$,

$$
2q_1,2q_2<N/3<2q_1q_2.
$$

### Why base $2$ is special enough here

The paper proves that for distinct safe primes greater than $5$,

$$
\operatorname{ord}_N(2)\in\{q_1q_2,2q_1q_2\}.
$$

Sketch: $\operatorname{ord}_N(2)=\operatorname{lcm}(\operatorname{ord}_{p_1}(2),\operatorname{ord}_{p_2}(2))$ and $\operatorname{ord}_{p_i}(2)\mid 2q_i$. Orders $1,2,q_i,2q_i$ are ruled out for $2$ modulo the other prime factor, leaving only $q_1q_2$ or $2q_1q_2$.

Given $r=\operatorname{ord}_N(2)\in\{q_1q_2,2q_1q_2\}$, QOFA returns $d=r/\gcd(t,r)$. The only useless outputs are $d=1$ or $d=2$, which occur with total probability $1/(q_1q_2)$. Every other possible $d$ contains $q_1$, $q_2$, or $q_1q_2$ enough to recover the factors.

---

## Key results

### Lemma 1: order structure and QOFA success probability

Let

$$
N=p_1p_2=(2q_1+1)(2q_2+1)
$$

be a safe semiprime with $q_1\neq q_2$ and $q_1,q_2>2$. Then:

1. $\operatorname{ord}_N(2)$ is either $q_1q_2$ or $2q_1q_2$.
2. One QOFA call on $(2,N)$ yields

   $$
   d\in\{q_1,q_2,q_1q_2,2q_1,2q_2,2q_1q_2\}
   $$

   with probability

   $$
   1-\frac{1}{q_1q_2}\sim 1-\frac{4}{N}.
   $$

   Otherwise $d\in\{1,2\}$.

### Main algorithmic consequence

For safe semiprimes $N=(2q_1+1)(2q_2+1)$ satisfying the paper's distinctness and size assumptions, Algorithm 1 factors $N$ with one QOFA call and success probability

$$
1-\frac{1}{q_1q_2}.
$$

If it fails, rerunning the same circuit for base $2$ gives another independent QOFA sample; no new randomly chosen base is needed.

### Standard Shor comparison on safe semiprimes

For a uniformly random base $a$, assuming perfect order recovery, the ordinary Shor wrapper succeeds on a safe semiprime with probability

$$
\Pr[r\text{ even}]\Pr[a^{r/2}\not\equiv -1\pmod N\mid r\text{ even}]
=\frac34\cdot\frac23=\frac12.
$$

With realistic QOFA output, which may be a proper divisor of the order, the paper states the success probability is at most $1/2$, so the expected number of QOFA calls is at least $2$.

---

## Comparison with prior work

| Method | Input class | Quantum calls (QOFA calls) | Success from one call | Classical post-processing |
|---|---:|---:|---:|---|
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Standard Shor factoring]] | General composite $N$ | Expected polynomial; for safe semiprimes expected $\ge 2$ QOFA calls | $\le 1/2$ for safe semiprimes | Use even order $r$ and compute $\gcd(a^{r/2}\pm1,N)$ |
| This paper | Safe semiprime $N=(2q_1+1)(2q_2+1)$ | $1$ QOFA call except with probability $1/(q_1q_2)$ | $1-1/(q_1q_2)\sim1-4/N$ | Recover $2q_i$ or $\phi(N)$ from the QOFA divisor $d$ |
| Generalised salvage tests in this paper | General composite $N$ | Same QOFA calls as Shor, possibly fewer reruns | Empirical improvement on $N\in[10,10^8]$ for fixed $a=2$ | Try $\gcd(r,N)$ and small-prime divisors $\ell\mid r$ before rerunning QOFA |

Compared with work that optimises QOFA circuits or base selection, this paper changes the classical interpretation of one QOFA sample. That is the right way to read the contribution: not faster modular exponentiation, but better extraction of factors from a divisor of an order.

---

## Limits / caveats

- The main algorithm is **only proved for safe semiprimes** with the stated distinctness assumptions. It does not extend cleanly even to $N=p_1p_2$ where only one prime is safe.
- The success proof assumes the idealised QOFA output $d=r/\gcd(t,r)$ after continued fractions. The paper notes that finite-QFT rounding can be handled by checking nearby values, as in [[Order-Finding via QFT and Continued Fractions]], but the stated theorem is about the idealised order-sampling model.
- The base $a=2$ is chosen without knowing the factors, which avoids the usual criticism of compiled demonstrations of Shor's algorithm. Still, the paper's practical advantage is mostly in repeated experiments: one fixed modular-exponentiation circuit can be reused.
- The claim that the algorithm is “never slower than SFA” should be read in the paper's QOFA-call model. If a standard Shor run lucks into $\gcd(a,N)>1$ before QOFA, it can succeed with no quantum call, but that event is negligible for cryptographic-size semiprimes.
- For general composites, the proposed add-ons are cheap and sensible, but not a replacement for Shor's algorithm. The evidence for those salvage tests is empirical and does not inherit the safe-semiprime theorem.

---

## Reusable ideas

1. **[[Safe-Semiprime Factoring from a QOFA Divisor]]:** If the factorisation pattern forces $\lambda(N)$ to have very few divisors, a divisor of an order may already encode $\phi(N)$ or one prime factor.
2. **[[Fixed-Base Order Finding for Reusable Factoring Circuits]]:** Use a factor-independent fixed base, here $a=2$, so reruns reuse the same modular-exponentiation circuit.
3. **[[Classical Salvage Tests for Shor Order Outputs]]:** Before paying for another QOFA call, test cheap consequences of a failed order sample: $\gcd(r,N)$, small prime divisors of $r$, and possible recovery of $\lambda(N)$.

---

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994/1997)]] — quantum order finding plus classical factoring wrapper; the baseline being improved.
- [[Order-Finding via QFT and Continued Fractions]] — the QOFA subroutine treated as a black box here.
- [[Quantum Fourier Transform Circuit]] — underlying QFT primitive in QOFA; approximate versions are cited as resource-saving variants.
- Nielsen and Chuang (2000) — standard textbook reference for QOFA and Shor's algorithm.
- Pollard (1974) — $p-1$ factoring; safe primes are hard for that classical method because $p-1=2q$ has a large prime factor.
- Lenstra (1987) — elliptic curve factorisation, which weakened the older motivation for strong primes.
- Cramer and Shoup (2000) — signature scheme using safe semiprimes under the strong RSA assumption.
- Leander (2002), Markov and Saeedi (2012), Lawson (2015) — prior work on improving Shor success probability/base choices.
- Knill (1995) — extracting more information from imperfect QOFA outputs and QFT modulus tradeoffs.
- Smolin, Smith, and Vargo (2013) — warning about over-compiled factoring demonstrations that choose bases using knowledge of the factors.
- Miller (1976) — factoring from $\phi(N)$, probabilistically in polynomial time, or deterministically under ERH.

---

## Cross-links

### Paper notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]]
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]]

### Trick cards

- [[Order-Finding via QFT and Continued Fractions]]
- [[Quantum Fourier Transform Circuit]]
- [[Reversible Modular Exponentiation with Garbage Cleanup]]
- [[Safe-Semiprime Factoring from a QOFA Divisor]]
- [[Fixed-Base Order Finding for Reusable Factoring Circuits]]
- [[Classical Salvage Tests for Shor Order Outputs]]
