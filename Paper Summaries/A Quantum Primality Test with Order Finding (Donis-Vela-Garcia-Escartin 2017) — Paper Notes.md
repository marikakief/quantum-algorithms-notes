# A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes

> **Source:** Alvaro Donis-Vela and Juan Carlos Garcia-Escartin, *A quantum primality test with order finding*, arXiv:1711.02616 (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1711.02616) · [PDF](https://arxiv.org/pdf/1711.02616)
> **Tags:** #quantum-algorithms #primality-testing #order-finding #number-theory

---

## The computational problem

**Input:** an odd integer $N$ with $2^{n-1}<N\le 2^n$.

**Output:** one of:

1. `prime`, with a quantum-checkable certificate $a\in\mathbb Z_N^*$ satisfying $\operatorname{ord}_N(a)=N-1$;
2. `composite`, with a classical witness: either a nontrivial factor $\gcd(a,N)$ or a Fermat/Euler-style witness; or
3. `composite with high probability` after enough failed attempts to find an element of order $N-1$.

**Complexity measure:** arithmetic and quantum-gate operations, dominated by calls to quantum order finding. The paper treats order finding as a black-box Shor subroutine and counts expected repetitions over random bases.

The test uses the converse of Fermat's theorem in Lucas form:

$$
\exists a\in\mathbb Z_N^*\text{ with }\operatorname{ord}_N(a)=N-1 \quad\Longrightarrow\quad N\text{ is prime}.
$$

The implication is simple but useful: $\operatorname{ord}_N(a)\mid \varphi(N)$, and for composite $N>1$ one has $\varphi(N)<N-1$.

## What the paper does

Donis-Vela and Garcia-Escartin propose a primality test that runs quantum order finding directly on random bases, rather than factoring $N-1$ first and then applying a Lucas--Lehmer certificate. For prime $N$, a random element of $\mathbb Z_N^*$ has order $N-1$ with probability $\varphi(N-1)/(N-1)$, so $O(\log\log N)=O(\log n)$ trials suffice with high probability.

My assessment: this is a neat algorithmic variant, not a new primitive. It saves the quantum factoring-of-$N-1$ stage used by Chau--Lo, but the certificate becomes quantum-checkable rather than classically checkable. That tradeoff is exactly where the paper is interesting.

The algorithm is one-sided as a prime-certification method: finding an order-$N-1$ base proves primality, while failing to find one after a chosen number of trials is only a probabilistic/computational stopping condition, not a proof that no certificate exists.

## The algorithm / construction

### Number-theoretic ingredients

The paper uses the following facts.

1. **Fermat/Euler:** if $N$ is prime and $a\in\mathbb Z_N^*$, then $a^{N-1}\equiv 1\pmod N$.
2. **Order divisibility:** for $a\in\mathbb Z_N^*$, $\operatorname{ord}_N(a)\mid \varphi(N)$.
3. **Lucas converse:** if $\operatorname{ord}_N(a)=N-1$, then $N$ is prime.
4. **Primitive elements in prime fields:** for prime $p$ and $d\mid p-1$, $\mathbb Z_p^*$ has exactly $\varphi(d)$ elements of order $d$. In particular, it has $\varphi(p-1)$ primitive roots.

The key probability estimate is

$$
\Pr_{a\leftarrow \mathbb Z_N^*}\left[\operatorname{ord}_N(a)=N-1\right]
=\frac{\varphi(N-1)}{N-1}
>\frac{1}{3\log\log(N-1)}
$$

for prime $N$ with $N-1>49.2$; the paper quotes the Rosser--Schoenfeld lower bound

$$
\frac{\varphi(M)}{M}>
\frac{1}{e^\gamma \log\log M + 2.50637/\log\log M}.
$$

### Main loop

For each trial:

1. Choose a random integer $1<a<N$.
2. Compute $g=\gcd(a,N)$.
   - If $g\ne 1$, output `composite` with factor $g$.
3. Compute

   $$
   b=a^{(N-1)/2}\bmod N.
   $$

4. If $b\not\equiv \pm 1\pmod N$, output `composite`; then $a$ is a Fermat-type witness because $a^{N-1}\not\equiv 1\pmod N$.
5. If $b\equiv 1\pmod N$, discard $a$ and repeat. In this case $\operatorname{ord}_N(a)\mid (N-1)/2$, so $a$ cannot certify primality.
6. If $b\equiv -1\pmod N$, run quantum order finding on $a\bmod N$.
7. If order finding returns $\operatorname{ord}_N(a)=N-1$, output `prime` and return $a$ as a quantum certificate.
8. Otherwise discard $a$ and repeat.

In flow form:

```text
random a
   |
   v
gcd(a,N) != 1 ?  ---> composite + factor
   |
   v
compute a^((N-1)/2) mod N
   |
   +-- not ±1 ---> composite + Fermat/Euler witness
   |
   +-- +1 ------> discard base; repeat
   |
   +-- -1 ------> quantum order finding
                         |
                         +-- ord(a)=N-1 ---> prime + quantum certificate a
                         |
                         +-- otherwise ----> discard base; repeat
```

The $a^{(N-1)/2}$ screening is the paper's main practical filter. It avoids sending obviously useless bases to the quantum subroutine. If the result is $-1$, then $a^{N-1}\equiv 1\pmod N$, so $\operatorname{ord}_N(a)\mid N-1$; at that point exact order finding is the right test.

### Miller--Rabin preselection variant

The authors also suggest replacing the simple half-exponent screen by a Miller--Rabin preselection stage. Write $N-1=2^s d$ with $d$ odd. For a prime $N$, every base satisfies

$$
a^d\equiv 1\pmod N
\quad\text{or}\quad
\exists r<s: a^{2^r d}\equiv -1\pmod N.
$$

For composite $N$, at most one quarter of bases in $\mathbb Z_N^*$ are strong liars. After $k$ random Miller--Rabin bases without a witness, the probability that a composite input has survived is bounded by $4^{-k}$. Bases with $a^d\equiv 1$ are discarded because they have too-small order; surviving bases are candidates for the order-finding call.

This survival bound is a composite-input rejection bound under independent random bases. It is separate from the prime-input probability of finding a primitive root/order-$N-1$ base. The preselection stage is best read as a way to spend cheap classical modular exponentiations before using the quantum machine.

### Prime-power side remark

The conclusion sketches an extension for detecting prime powers. Primitive roots modulo $N$ exist only for

$$
N\in\{2,4,p^k,2p^k\}.
$$

For $N=p^k$,

$$
\varphi(N)=p^{k-1}(p-1).
$$

If order finding finds a primitive root with order $\varphi(N)$, then

$$
\gcd(\operatorname{ord}_N(a),N)=p^{k-1},
$$

and repeated division by

$$
p=\frac{N}{\gcd(\operatorname{ord}_N(a),N)}
$$

checks the prime-power structure in polynomial time. This is not the main algorithm, but it is a useful observation for pre-processing inputs before Shor factoring.

## Key results

### Lucas-order primality certificate

If the algorithm finds $a\in\mathbb Z_N^*$ with

$$
\operatorname{ord}_N(a)=N-1,
$$

then $N$ is prime. The certificate is the base $a$, but verification requires a quantum order-finding machine unless one also supplies a classical factorisation of $N-1$.

### Success probability on prime inputs

For prime $N$, exactly $\varphi(N-1)$ elements of $\mathbb Z_N^*$ have order $N-1$, so one random base succeeds with probability

$$
\frac{\varphi(N-1)}{N-1}.
$$

For large enough $N$,

$$
\frac{\varphi(N-1)}{N-1}>\frac{1}{3\log\log(N-1)}.
$$

Thus $O(\log\log N)=O(\log n)$ random trials give high probability of finding a primality certificate for prime $N$.

### Complexity

The paper quotes quantum order finding with ordinary multiplication as

$$
O((\log n)n^3)
$$

operations per order-finding call, where the $O(\log n)$ factor is the expected number of repetitions needed to reconstruct the exact order from period samples. With $O(\log n)$ candidate bases, the full expected cost is

$$
O((\log n)^2 n^3).
$$

Using fast multiplication asymptotically, the order-finding subroutine becomes

$$
O(\log\log n(\log n)^2 n^2),
$$

and the full expected cost becomes

$$
O(\log\log n(\log n)^3 n^2).
$$

The authors also note that classical post-processing in order finding can reduce the repeated period-sample factor from $O(\log n)$ to a constant in Shor's analysis, but they keep the quoted order-finding cost for the headline bounds.

## Comparison with prior work

| Method | Proves primality? | Quantum ingredient | Certificate | Complexity as discussed here | Comment |
|---|---:|---|---|---|---|
| Fermat / Solovay--Strassen / Miller--Rabin | no, except compositeness witnesses | none | compositeness witness when found | polynomial per trial; one-sided error | Fast filters, but cannot certify primes without extra assumptions or special cases. |
| AKS | yes | none | essentially $N$ plus deterministic verification | about sixth power in bitlength in the paper's discussion | Classical and unconditional, but slower in practice than probabilistic tests/ECPP. |
| Lucas--Lehmer / Pratt-style certificates | yes | none | factorisation chain for $N-1$ | fast if $N-1$ factors conveniently | Classical certificate, but needs arithmetic structure. |
| Chau--Lo quantum primality via factoring | yes | quantum factoring of $N-1$ plus Lucas--Lehmer | classical factorisation data for $N-1$ | $O(n^3\log n\log\log n)$ expected operations as quoted | Stronger certificate story; more quantum work. |
| Donis-Vela--Garcia-Escartin | yes | direct [[Order-Finding via QFT and Continued Fractions|order finding]] on random bases | base $a$ with $\operatorname{ord}_N(a)=N-1$, quantum-checkable | $O((\log n)^2n^3)$, or $O(\log\log n(\log n)^3n^2)$ with fast multiplication | Saves factoring $N-1$, but gives up a classical certificate. |

The complexity entries in the quantum rows use the paper's arithmetic/order-finding model; they are not modern fault-tolerant modular-exponentiation resource estimates. A base $a$ alone is a quantum-checkable Lucas certificate unless enough classical factorisation data about $N-1$ are supplied.

## Limits / caveats

- The positive certificate is not classically checkable unless one also supplies enough information about $N-1$. A verifier without a quantum computer cannot efficiently confirm $\operatorname{ord}_N(a)=N-1$ in general.
- For composite $N$, the order test may be inconclusive rather than giving a witness. Carmichael numbers are the obvious bad family for Fermat-style screens: all coprime bases satisfy $a^{N-1}\equiv 1\pmod N$, although their orders are still $<N-1$.
- The headline speedup over Chau--Lo is partly certificate tradeoff, not a stronger primality-theory result. If classical certificates matter, Chau--Lo or ECPP-style methods remain cleaner.
- The fast-multiplication bound is asymptotic. The paper explicitly notes that constant factors in fast modular multiplication make it worthwhile only for very large inputs.
- The paper has a small phrasing issue around "log log N unsuccessful attempts" versus the later $3\log n$ stopping discussion for composite inputs. The probability estimate for primitive roots gives $O(\log\log N)=O(\log n)$ attempts on prime inputs; for composite rejection the guarantee is tied to the chosen screening test, e.g. Miller--Rabin's $4^{-k}$ bound.

## Reusable ideas

1. [[Lucas Certificate by Direct Quantum Order Finding]] — certify primality by finding a base whose multiplicative order is exactly $N-1$.
2. [[Euler-Liar Screening Before Quantum Order Finding]] — spend classical modular exponentiations to filter bases before running the quantum period-finding circuit.
3. [[Primitive-Root Prime-Power Detection by Order GCD]] — use the order of a primitive root modulo $p^k$ to extract $p^{k-1}$ by a gcd with $N$.

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994/1997)]] — source of the order-finding subroutine and the complexity model for modular exponentiation plus QFT.
- [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes|Cheng (2003)]] — another primality-proving note in this Zoo batch; classical rather than quantum, but useful comparison for certificate tradeoffs.
- [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes|Morain (2005)]] — ECPP implementation context for classical primality certificates.
- [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes|Chau and Lo (1997)]] — quantum primality test via quantum factorisation of $N-1$ and Lucas--Lehmer; the main comparison target.
- Carlini and Hosoya (2000) — quantum counting/period-finding approach to Miller--Rabin-style testing.
- Lucas (1878) and Lehmer (1927) — converse-Fermat primality criteria behind the certificate.
- Rosser--Schoenfeld (1962) and Nicolas (1983) — lower and extremal estimates for $\varphi(M)/M$ used to bound repetitions.
- Agrawal--Kayal--Saxena (2004), Lenstra--Pomerance (2003) — deterministic classical primality testing baseline.
- Vedral--Barenco--Ekert (1996), Beckman--Chari--Devabhaktuni--Preskill (1996), Zalka (1998), Van Meter--Itoh (2005) — arithmetic-circuit cost references for modular exponentiation.
- Bernstein (1998) — classical detection of perfect powers, mentioned as an alternative to the prime-power order trick.

## Cross-links

### Paper notes

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Primality Test Via Quantum Factorization (Chau-Lo 1996) — Paper Notes]]
- [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]]
- [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes]]
- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]]

### Trick cards

- [[Order-Finding via QFT and Continued Fractions]]
- [[Lucas Certificate by Direct Quantum Order Finding]]
- [[Euler-Liar Screening Before Quantum Order Finding]]
- [[Primitive-Root Prime-Power Detection by Order GCD]]
