# Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes

> **Source:** François Morain, *Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm*, arXiv:math/0502097, Mathematics of Computation 76, 493–505 (2007)
> **Links:** [arXiv](https://arxiv.org/abs/math/0502097) · [PDF](https://arxiv.org/pdf/math/0502097)
> **Tags:** #primality-proving #ECPP #number-theory #classical-algorithms

---

## The computational problem

**Input:** an integer $N$ that is believed to be prime.

**Output:** an elliptic-curve primality certificate proving that $N$ is prime, or a factor/compositeness witness if the construction fails in a way that reveals compositeness.

**Cost measure:** heuristic bit complexity in terms of

$$
L=\log N,
$$

with integer multiplication on $L$-bit integers assumed to cost $O(L^{1+\mu})$, $0\le \mu\le 1$, and degree-$d$ polynomial multiplication modulo $N$ assumed to cost $O(d^{1+\nu}L^{1+\mu})$, $0\le \nu\le 1$.

This is a classical algorithm note, not a quantum algorithm. It belongs in the Zoo batch because it is part of the number-theoretic algorithm chain around primality proving and is cited directly by [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]].

## What the paper does

Morain gives the implementation-level version of the fastECPP idea attributed to Shallit: replace the basic ECPP search over $O(L^2)$ discriminants, each requiring a modular square-root computation, by a pool of $O(L)$ precomputed square roots whose pairwise products generate $O(L^2)$ candidate discriminants.

The claimed asymptotic change is from heuristic

$$
\tilde O(L^{5+\mu})
$$

for ordinary ECPP to heuristic

$$
\tilde O(L^{4+\mu})
$$

for fastECPP. My assessment: the asymptotic idea is clean, but this paper is at least as much about making ECPP usable as it is about the exponent. The really practical contributions are the discriminant ordering, class-number filtering, Cornacchia shortcuts, and class-invariant engineering.

## The algorithm / construction

### Basic ECPP skeleton

For an integer $N$, ECPP searches for an imaginary quadratic field $K=\mathbb Q(\sqrt{-D})$ such that

$$
4N=U^2+DV^2
$$

for integers $U,V$. For each solution $U$, it considers

$$
m=N+1-U.
$$

In the simplified exposition in the paper, the goal is to find

$$
m=2N',
$$

where $N'$ is a probable prime. Then:

Real ECPP is more flexible than this pedagogical `2N'` case: one may use group orders of the form $cN'$ where the cofactor $c$ is sufficiently smooth and fully factored, with $N'$ the next probable-prime target.

1. Compute the Hilbert class polynomial

   $$
   H_D(X)=\prod_{(A,B,C)\in\mathrm{Cl}(-D)}\left(X-j\left(\frac{-B+\sqrt{-D}}{2A}\right)\right).
   $$

2. Find a root of $H_D(X)$ modulo $N$, giving a CM elliptic curve $E$ over $\mathbb Z/N\mathbb Z$.
3. Find a point $P=(x:y:1)$ on $E$ with $\gcd(y,N)=1$ such that

   $$
   \psi_{2N'}(x,y)=0,
   \qquad
   \gcd(\psi_{N'}(x,y),N)=1,
   $$

   where $\psi_m$ is the division polynomial used to define $[m]P$ over $\mathbb Z/N\mathbb Z$.
4. Recurse on $N'$.

The primality lift uses Hasse's theorem. If $N$ were composite and $p\le \sqrt N$ were a prime factor, reducing modulo $p$ would give a point of order $2N'$ on $E(\mathbb F_p)$, contradicting the Hasse bound under the size condition

$$
(\sqrt N-1)^2\le 2N'\le (\sqrt N+1)^2.
$$

### Where ordinary ECPP spends its time

For each discriminant $D$, solving $4N=U^2+DV^2$ needs a square root

$$
\sqrt{-D}\pmod N,
$$

costing one modular exponentiation, $O(L^{2+\mu})$, plus a Cornacchia/half-gcd reduction costing $O(L^{1+\mu})$. Under the paper's heuristics, $O(L^2)$ discriminants are tried per ECPP level and $O(L)$ probable-prime tests are made, so Step 1 alone costs

$$
O(L^2L^{2+\mu})+O(LL^{2+\mu})=O(L^{4+\mu})
$$

per level. With $O(L)$ levels, ordinary ECPP is heuristic $O(L^{5+\mu})$ total.

### fastECPP: build many discriminants from few square roots

The fast version replaces the discriminant search step by:

1. Find

   $$
   r=O(L)
   $$

   small signed prime discriminant factors $q^*$ with

   $$
   \left(\frac{q^*}{N}\right)=1.
   $$

2. Compute all square roots $\sqrt{q^*}\bmod N$ for $q^*$ in the pool

   $$
   Q=\{q_1^*,\ldots,q_r^*\}.
   $$

3. For pairs, or more generally subsets, form fundamental discriminants

   $$
   -D=q_{i_1}^*q_{i_2}^*
   $$

   in the pair version, and obtain $\sqrt{-D}\bmod N$ by multiplying the already-computed square roots.
4. Run Cornacchia's algorithm to test whether $4N=U^2+DV^2$ is solved.
5. If $m=N+1\pm U$ has the right form $cN'$ with smooth $c$ and probable-prime $N'$, build the CM curve and the point certificate, then recurse.

The asymptotic accounting is the core idea:

$$
O(LL^{2+\mu}) \quad \text{for square roots}
$$

plus

$$
O(L^2L^{1+\mu}) \quad \text{for Cornacchia reductions}
$$

plus

$$
O(LL^{2+\mu}) \quad \text{for probable-prime tests},
$$

so the discriminant phase becomes

$$
O(L^{3+\mu})
$$

per recursion level. Since there are $O(L)$ levels, fastECPP gives heuristic

$$
O(L^{4+\mu})
$$

total time.

ASCII view of the replacement:

```text
ordinary ECPP:
  D_1, D_2, ..., D_{L^2}
      each requires sqrt(-D_i) mod N

fastECPP:
  q_1^*, ..., q_L^*
      compute sqrt(q_i^*) mod N once
      combine products q_i^* q_j^* to get about L^2 discriminants
```

### Implementation details that matter

Morain then turns the asymptotic sketch into a working program:

- **Class-number control.** The program estimates or computes $h(-D)$ before committing to $D$. For the medium-sized $D$ used in practice, Louboutin's explicit formula gives an $O(h)$ method with a small constant.
- **Delayed multiprecision in Cornacchia.** Since a random $D$ succeeds with probability about $1/(2h(-D))$, most Cornacchia calls fail. The implementation tracks the $w_i$ recurrence modulo $2^{32}$ first, and only reruns the full multiprecision recurrence if the final congruence survives the residue test.
- **Factoring $m=N+1\pm U$.** The algorithm wants $m=cN'$ with $c$ smooth and $N'$ probable prime. For small inputs, trial/sieving against primes $p_i\le B$ gives cost

  $$
  O(tBL)+O(tL^{2+\mu})=O(BL^2)+O(L^{3+\mu}),
  $$

  with optimal $B=O(L)$ in that model. For larger inputs, Morain points to the stripping-factor method of Franke--Kleinjung--Morain--Wirth, with optimal $B=O((\log N)^3)$.
- **Early abort.** Only test $N'$ for probable primality if $N/N'$ is above a chosen bound; the implementation used $N/N'\ge 2^\delta$.
- **Small class polynomials.** Replacing $j$ by better class invariants does not change the asymptotic complexity, but it is decisive for the implementation because it shrinks the class polynomials.
- **Smooth class numbers for modular root finding.** Step 3 factors $H_D(X)$ modulo $N$. If $h(-D)$ has small prime factors, Galois-theoretic decomposition replaces one degree-$h$ problem by smaller ones.

## Key results

### ECPP correctness proposition

Let $N'$ be prime and suppose

$$
(\sqrt N-1)^2\le 2N'\le (\sqrt N+1)^2.
$$

If there is an elliptic curve $E(\mathbb Z/N\mathbb Z)$ and a point $P=(x:y:1)$ with $\gcd(y,N)=1$ such that

$$
\psi_{2N'}(x,y)=0
$$

and

$$
\gcd(\psi_{N'}(x,y),N)=1,
$$

then $N$ is prime.

### Ordinary ECPP heuristic cost

Under the paper's discriminant and curve-order heuristics, taking $D_{\max}=O(L^2)$ gives $h(-D)=O(L)$ and makes the basic discriminant search cost

$$
O(L^{4+\mu})
$$

per ECPP level. With $O(L)$ levels, the total heuristic cost is

$$
O(L^{5+\mu}).
$$

### fastECPP heuristic cost

Using a pool of $O(L)$ square roots to generate $O(L^2)$ discriminants lowers the discriminant phase to

$$
O(L^{3+\mu})
$$

per level. The total heuristic cost becomes

$$
O(L^{4+\mu}).
$$

The paper notes that Step 3, reducing the CM curve by finding a modular root of $H_D$, has cost

$$
O((\log L)L^{3+\mu+\nu}),
$$

so after the square-root bottleneck is removed, several phases are close enough that pushing below $\tilde O(L^4)$ would require more than one local optimisation.

### Practical timings

The implementation was tested on the first twenty primes of $1000$, $1500$, and $2000$ decimal digits. Average total times on the reported AMD Athlon 64 3400+ were:

| Decimal digits | Average total CPU time | Average certificate-check time | Average recursion steps |
|---:|---:|---:|---:|
| 1000 | 334 s | 20 s | 143 |
| 1500 | 1701 s | 85 s | 198 |
| 2000 | 5557 s | 238 s | 248 |

Morain reports that the averages track the $O((\log N)^4)$ prediction closely. The most visible practical costs in the tables are probable-prime tests and modular root finding for the class polynomial.

These timings are historical scaling evidence for the implementation and hardware of the paper, not current performance guidance.

## Comparison with prior work

| Method | Claimed/known prover time | Verification/certificate picture | Main bottleneck |
|---|---:|---|---|
| AKS | deterministic polynomial; original quoted as $\tilde O(L^{10.5})$, later Lenstra--Pomerance announced $\tilde O(L^6)$ | no short certificate in the ECPP sense | polynomial congruence arithmetic |
| Berrizbeitia/Bernstein/Mihăilescu--Mocenigo variants | claimed probabilistic $\tilde O(L^4)$ in the paper's discussion | classical primality proving via cyclotomic/Jacobi-sum ideas | arithmetically restricted cases and distribution assumptions |
| Basic ECPP | heuristic $O(L^{5+\mu})$ in Morain's analysis | recursive elliptic-curve certificate; fast to check | $O(L^2)$ modular square roots per level |
| fastECPP | heuristic $O(L^{4+\mu})$ | same certificate style as ECPP | class-polynomial root finding, probable-prime tests, and remaining $O(L^3)$-scale subroutines |
| [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes|Cheng 2003]] | heuristic $\tilde O(L^4)$ | one ECPP reduction plus one AKS/Berrizbeitia-style congruence | good-prime density in short intervals |

## Limits / caveats

- The runtime is heuristic. Morain is explicit that the probability of finding suitable $m=N+1\pm U$ of the desired form is not proved with current analytic number theory.
- Completed ECPP certificates are deterministic to verify once the certificate data exist. The heuristic status concerns expected prover time and search success, not the validity of a produced certificate.
- The paper does not make ECPP a single fully specified algorithm; the choice and ordering of discriminants are implementation choices.
- Composite discriminants built from products of $q_i^*$ have even class number by genus theory. Morain conjectures the resulting bias is not important, but it is not ruled out.
- Better class invariants reduce constants and memory, not the asymptotic exponent.
- This is classical background. It matters for quantum-algorithm comparisons because factoring/order-finding papers often compare against classical primality and factoring technology, but there is no quantum speedup here.

## Reusable ideas

1. [[Quadratic-Residue Basis Discriminant Pool for fastECPP]] — precompute square roots for a small signed-prime basis and combine them to test many CM discriminants.
2. [[Delayed Multiprecision Cornacchia Screening]] — run the likely-failing Cornacchia recurrence modulo a machine word before paying for full multiprecision arithmetic.
3. [[Smooth Class-Number Filtering for CM Polynomial Splitting]] — prefer CM discriminants whose class numbers make the modular class-polynomial root step split into smaller subproblems.

## References within this paper

- Atkin--Morain (1993) — main ECPP reference and source of many implementation details.
- Goldwasser--Kilian (1999) — elliptic-curve primality testing background.
- Lenstra--Lenstra (1990) — number-theory algorithms handbook chapter where the Shallit fastECPP idea appears.
- Franke--Kleinjung--Morain--Wirth (2004) — large-number fastECPP implementation and distributed proving; cited for stripping factors and records.
- Enge--Morain (2002) and Enge (2004) — class invariants and class-polynomial computation.
- Couveignes--Henocq (2002) and Bröker--Stevenhagen (2004) — class-polynomial computation with GRH-based guarantees.
- Louboutin (2002) — explicit class-number formula used for practical $h(-D)$ computation.
- [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes|Cheng (2003)]] — cited in the conclusion as an application that asks ECPP to find an $N'$ with an extra medium-prime-factor condition.

## Cross-links

### Paper notes

- [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]]
- [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]]

### Trick cards

- [[Quadratic-Residue Basis Discriminant Pool for fastECPP]]
- [[Delayed Multiprecision Cornacchia Screening]]
- [[Smooth Class-Number Filtering for CM Polynomial Splitting]]
- [[Medium-Prime-Factor Targeting by One ECPP Round]]
