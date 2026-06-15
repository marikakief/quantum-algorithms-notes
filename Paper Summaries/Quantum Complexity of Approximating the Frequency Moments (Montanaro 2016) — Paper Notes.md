> **Source:** Ashley Montanaro, *The quantum complexity of approximating the frequency moments*, Quantum Information and Computation 16(13&14):1169–1190 (2016); arXiv:1505.00113
> **Links:** [arXiv](https://arxiv.org/abs/1505.00113) · [PDF](https://arxiv.org/pdf/1505.00113)
> **Tags:** #quantum-algorithms #query-complexity #streaming #frequency-moments #amplitude-estimation #k-distinctness

---

## The computational problem

Given a stream or oracle string

$$
a_1,\ldots,a_n\in[m],
$$

let $n_j=|\{i:a_i=j\}|$. The $k$th frequency moment is

$$
F_k=\sum_j n_j^k,
$$

with $F_0$ the number of distinct elements and

$$
F_\infty=\max_j n_j.
$$

The task is to output $\widetilde F_k$ such that

$$
\Pr[|\widetilde F_k-F_k|>\epsilon F_k]\le 1/3.
$$

Montanaro studies two access models:

- **Query model:** coherent oracle access
  $$
  O|i\rangle|x\rangle=|i\rangle|x+a_i\rangle,
  $$
  and the goal is to minimise input queries.
- **Multi-pass streaming model:** each pass processes $a_1,\ldots,a_n$ sequentially with limited quantum memory; the costs are space $S$ and number of passes $T$.

## What the paper does

This paper gives quantum algorithms for approximating frequency moments with up to quadratic improvements over classical query and streaming bounds. The cleanest query-model result is $F_0$ in $O(\sqrt n/\epsilon)$ queries, which is tight. For $k\ge2$, the algorithm combines classical collision-count estimators with Belovs' $k$-distinctness quantum walk to beat the classical $\Omega(n^{1-1/k}/\epsilon^2)$ sample lower bound in the exponent of $n$.

My assessment: the query-model part is the more structurally interesting piece. The streaming separations are also real, but they trade one resource for another — many passes with tiny quantum memory — so the right comparison is the time-space product $TS$, not just memory.

## The algorithm / construction

### 1. Query algorithm for $F_0$

The algorithm imports the Bar-Yossef--Jayram--Kumar--Sivakumar--Trevisan distinct-elements estimator. Pick a pairwise independent hash function

$$
h:[m]\to[M],\qquad M=m^3,
$$

and find the

$$
d=\lceil 96/\epsilon^2\rceil
$$

smallest **distinct** values among $h(a_i)$. If $v$ is the $d$th smallest value, output

$$
\widetilde F_0=dM/v.
$$

The quantum part is D\"urr--Heiligman--H\o yer--Mhalla's algorithm for finding the $d$ smallest values of different types in

$$
O(\sqrt{dn})=O(\sqrt n/\epsilon)
$$

queries.

### 2. Query algorithm for $F_k$, $k>1$

The classical starting point is Bar-Yossef's observation that $F_k$ can be estimated by counting $k$-wise collisions in random subsequences. For random indices $s_1,\ldots,s_\ell\in[n]$, define

$$
C_k(s_1,
\ldots,s_\ell)=\left|\left\{T\in\binom{[\ell]}{k}:a_{s_i}=a_{s_j}\text{ for all }i,j\in T\right\}\right|.
$$

The key moment bounds are

$$
\mathbb E[C_k]=\binom{\ell}{k}\frac{F_k}{n^k},
$$

and

$$
\operatorname{Var}(C_k)\le \sum_{q=k}^{2k-1}\left(\frac{\ell F_k^{1/k}}{n}\right)^q.
$$

The algorithm:

1. Try subsequence sizes $2^i$ until Belovs' $k$-distinctness algorithm finds a $k$-collision. This chooses
   $$
   \ell=\Theta(n/F_k^{1/k})
   $$
   with good probability.
2. Repeat $M=\Theta(1/\epsilon^2)$ times:
   - sample a fresh subsequence $S$ of length $\ell$;
   - repeatedly run $k$-distinctness on $S$ after deleting one representative from each found collision class;
   - reconstruct a surrogate sequence $B$ with the same discovered equality classes;
   - compute $C_k(B)$ classically.
3. Output
   $$
   \frac{n^k}{M\binom{\ell}{k}}\sum_{r=1}^M C(r).
   $$

Belovs' $k$-distinctness cost is

$$
O\!\left(N^{\alpha_k}\log(1/\delta)\right),
\qquad
\alpha_k=1-\frac{2^{k-2}}{2^k-1}.
$$

Substituting $N=\ell=\Theta(n/F_k^{1/k})$ and using $F_k\ge n$ gives the final query bound.

### 3. Query lower bounds

The lower bounds come from standard query problems:

- $F_0$: threshold gives
  $$
  \Omega(\sqrt{n/\epsilon}).
  $$
- $F_k$, $k>1$: approximate counting gives
  $$
  \Omega(n^{1/2-1/(2k)}/\epsilon),
  $$
  and small-range collision gives
  $$
  \Omega(n^{1/3}).
  $$
- $F_\infty$: element distinctness gives
  $$
  \Omega(n^{2/3}),
  $$
  and approximate counting gives $\Omega(1/\epsilon)$.
- Very high precision: for any $k\ne1$, $O(1/n)$ relative error costs $\Theta(n)$ queries.

### 4. Streaming algorithm for $F_0$

The streaming algorithm estimates the probability that a random hash function maps at least one stream item to $1$. Given a rough estimate $R=\Theta(F_0)$ and a $t$-wise independent family

$$
h_j:[m]\to[R],\qquad t=\Theta(\log(1/\epsilon)),
$$

let

$$
p=\Pr_j[\exists i:h_j(a_i)=1].
$$

If $p$ is estimated to additive error $O(\epsilon)$, then

$$
\widetilde F_0=\frac{\ln(1-\widetilde p)}{\ln(1-1/R)}
$$

is a relative-error estimate of $F_0$.

The quantum step is [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude estimation]]. The predicate $[\exists i:h_j(a_i)=1]$ is made reversible with two passes: one pass accumulates

$$
N_j=|\{i:h_j(a_i)=1\}|,
$$

then a flag records whether $N_j\ne0$, and a second pass uncomputes $N_j$.

### 5. Streaming algorithm for $F_2$

Use the Alon--Matias--Szegedy estimator. For a 4-wise independent hash function

$$
h:[m]\to\{\pm1\},
$$

define

$$
f(h)=\left(\sum_{i=1}^n h(a_i)\right)^2.
$$

Then

$$
\mathbb E[f]=F_2,\qquad \operatorname{Var}(f)\le2F_2^2.
$$

Each $f(h)$ is computed reversibly with two passes: accumulate $\sum_i h(a_i)$, add its square to the output register, then uncompute. Montanaro then applies his [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|bounded-relative-variance mean-estimation]] algorithm.

### 6. Streaming algorithm for $F_\infty$

For each candidate value $j$, a streaming pass can compute $n_j$ reversibly. Use [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|D\"urr--H\o yer maximum finding]] over $j$ to output

$$
F_\infty=\max_j n_j
$$

exactly with $O(\sqrt n)$ passes and $O(\log^2 n)$ qubits when the effective universe is $O(n)$.

## Key results

**Theorem 2.** $F_0$ can be approximated in the query model using

$$
O(\sqrt n/\epsilon)
$$

queries, with success probability at least $3/5-1/m$. The bound is tight by Theorem 6.

**Theorem 5.** For fixed $k>1$, $F_k$ can be approximated in the query model using expected

$$
O\!\left(\frac{n^{(1-1/k)(1-2^{k-2}/(2^k-1))}}{\epsilon^2}\log(n/\epsilon)\right)
$$

queries. For $k=2$, this is

$$
\widetilde O(n^{1/3}/\epsilon^2),
$$

matching the $\Omega(n^{1/3})$ dependence on $n$ up to logarithms for constant $\epsilon$.

**Theorem 6.** Query lower bounds:

$$
Q(F_0)=\Omega(\sqrt{n/\epsilon}),
$$

for $k>1$,

$$
Q(F_k)=\Omega(n^{1/2-1/(2k)}/\epsilon),\qquad Q(F_k)=\Omega(n^{1/3}),
$$

and

$$
Q(F_\infty)=\Omega(n^{2/3}),\qquad Q(F_\infty)=\Omega(1/\epsilon).
$$

**Theorem 7.** For any $k\ne1$, computing $F_k$ to $O(1/n)$ relative error requires $\Theta(n)$ quantum queries.

**Theorem 11.** In the multi-pass quantum streaming model, $F_0$ can be approximated using

$$
O(\log m\log(1/\epsilon)+\log n)
$$

qubits and

$$
O(1/\epsilon)
$$

passes.

**Theorem 14.** $F_2$ can be approximated in streaming using

$$
O(\log n+
\log(1/\epsilon))
$$

qubits and

$$
O\!\left(\frac{1}{\epsilon}\log^{3/2}(1/\epsilon)\log\log(1/\epsilon)\right)
$$

passes.

**Theorem 16.** $F_\infty$ can be computed exactly in streaming using

$$
O(\log^2 m)
$$

qubits and

$$
O(\sqrt n)
$$

passes.

**Theorem 23.** For $k\ne1$ and $\epsilon\ge1/\sqrt m$, any $T$-pass quantum streaming algorithm with $S$ qubits must satisfy

$$
TS=\Omega(1/\epsilon).
$$

## Comparison with prior work

| Result | Classical | Quantum in this paper |
|---|---:|---:|
| Query $F_0$ | $\Omega(n)$ for constant $\epsilon$ | $\Theta(\sqrt n/\epsilon)$ |
| Query $F_k$, $k\ge2$ | $\Omega(n^{1-1/k}/\epsilon^2)$ samples | $\widetilde O(n^{(1-1/k)(1-2^{k-2}/(2^k-1))}/\epsilon^2)$ |
| Query $F_2$ | $\Omega(n^{1/2}/\epsilon^2)$ | $\widetilde O(n^{1/3}/\epsilon^2)$ |
| Streaming $F_0$ | $TS=\Omega(1/\epsilon^2)$ classically | $S=O(\log m\log(1/\epsilon)+\log n)$, $T=O(1/\epsilon)$ |
| Streaming $F_2$ | $TS=\Omega(1/\epsilon^2)$ classically | $S=O(\log n+\log(1/\epsilon))$, $T=\widetilde O(1/\epsilon)$ |
| Streaming $F_\infty$ | $TS=\Omega(n)$ for large universe | $S=O(\log^2 m)$, $T=O(\sqrt n)$ |

## Limits / caveats

- The $F_k$ query upper bound inherits Belovs' $k$-distinctness exponent. Improvements to $k$-distinctness would immediately improve the frequency-moment algorithm.
- The $F_k$ query algorithm has $1/\epsilon^2$ dependence because the estimator is averaged classically. Montanaro explicitly notes that replacing this with quantum mean estimation might improve it to roughly $\widetilde O(1/\epsilon)$, but the runtime is only controlled in expectation.
- The streaming algorithms reduce space dramatically only when many passes are allowed. The honest comparison is the product $TS$.
- The $F_k$ streaming extension for $k>2$ is weak: it can beat classical algorithms only in very small-$\epsilon$ regimes.
- The exact quantum query complexity of approximating $F_\infty$ remains open and is tied to gapped $k$-distinctness.

## Reusable ideas

1. [[Hash-Minima Distinct-Count Estimation with Quantum Smallest-Values Search]] — estimate $F_0$ by quantumly finding the first $O(1/\epsilon^2)$ distinct hash minima.
2. [[Random-Subset Collision Counting for Frequency Moments]] — express $F_k$ as the expected number of $k$-collisions in a random subsequence, then use a quantum $k$-distinctness routine to enumerate collisions.
3. [[Reversible Multi-Pass Streaming Predicate for Amplitude Estimation]] — implement existential stream predicates reversibly using an accumulate-flag-uncompute pass structure.
4. [[Bounded-Variance Streaming Estimators via Quantum Mean Estimation]] — apply bounded-relative-variance quantum mean estimation to AMS-style streaming estimators.
5. [[Streaming Maximum Finding via Frequency Oracles]] — turn sequential passes into coherent frequency-count queries and run quantum maximum finding.
6. [[Frequency-Moment Lower Bounds by Query and Communication Reductions]] — reduce from threshold, approximate counting, collision, element distinctness, Gap-Hamming, Equality, and Disjointness to pin down lower bounds.

## References within this paper

- Bar-Yossef et al. (2002) — distinct-elements estimator based on smallest hash values; basis of the $F_0$ query and streaming algorithms.
- Alon--Matias--Szegedy (1996) — classical frequency moment streaming algorithms, including the $F_2$ estimator.
- [[Quantum Algorithm for k-Distinctness with Prior Knowledge on the Input (Belovs-Lee 2011) — Paper Notes|Belovs (2012)]] — learning-graph $k$-distinctness algorithm used inside the $F_k$ query algorithm.
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2004/2007)]] — element distinctness lower-bound and algorithmic context.
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes|Beals et al. (1998/2001)]] — threshold lower bound used for $F_0$.
- [[The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999) — Paper Notes|Nayak--Wu (1999)]] — approximate counting / polynomial-method lower bounds.
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard--Høyer--Mosca--Tapp (2002)]] — amplitude estimation in streaming $F_0$.
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — bounded-relative-variance mean estimation for streaming $F_2$.
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|D\"urr--H\o yer (1996)]] — maximum finding for streaming $F_\infty$.
- Chakrabarti--Regev (2012), Razborov (2003), Klauck--Špalek--de Wolf (2007) — communication lower-bound chain for streaming $TS=\Omega(1/\epsilon)$.

## Cross-links

### Paper notes

- [[Quantum Algorithm for k-Distinctness with Prior Knowledge on the Input (Belovs-Lee 2011) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]

### Trick cards

- [[Hash-Minima Distinct-Count Estimation with Quantum Smallest-Values Search]]
- [[Random-Subset Collision Counting for Frequency Moments]]
- [[Reversible Multi-Pass Streaming Predicate for Amplitude Estimation]]
- [[Bounded-Variance Streaming Estimators via Quantum Mean Estimation]]
- [[Streaming Maximum Finding via Frequency Oracles]]
- [[Frequency-Moment Lower Bounds by Query and Communication Reductions]]
