> **Source:** Ashley Montanaro, *Quantum pattern matching fast on average*, Algorithmica 77:16–39 (2017); arXiv:1408.1816
> **Links:** [arXiv](https://arxiv.org/abs/1408.1816) · [PDF](https://arxiv.org/pdf/1408.1816)
> **Tags:** #quantum-algorithms #pattern-matching #hidden-shift #average-case #query-complexity #amplitude-amplification

---

## The computational problem

The $d$-dimensional pattern matching problem takes oracle access to

$$
T:[n]^d\to\Sigma,\qquad P:[m]^d\to\Sigma,
$$

with $m\le n$. The task is to find an offset $s\in[n-m]^d$ such that

$$
T(s+x)=P(x)\quad \text{for all }x\in[m]^d,
$$

or report that no match exists. The paper studies both a structured promise version and the average-case version where $T$ is uniformly random and $P$ is either a random substring of $T$ or an independent random pattern.

Two parameters drive the structured result:

- **Injectivity length** $\upsilon(S)$: the smallest $k$ such that the block string $S^{\triangleright k}$, whose symbols are $k^d$-blocks of $S$, is injective.
- **Local injectivity length** $\upsilon(S,m)$: the smallest $k$ such that every $m^d$-window of $S^{\triangleright k}$ is injective.

There is also a separation parameter $\gamma$: every non-match offset must disagree with the pattern on at least a $\gamma$ fraction of positions.

## What the paper does

Montanaro gives an average-case quantum pattern-matching algorithm with runtime

$$
\widetilde O\!\left((n/m)^{d/2}2^{O(d^{3/2}\sqrt{\log m})}\right),
$$

for random texts and random or planted patterns. The quantum speedup comes from reducing pattern matching to a hidden-shift problem on injectivised substrings, then searching over coarse offsets with bounded-error [[Standard Amplitude Amplification]].

My assessment: this is a clean average-case separation, but it is not a practical string-matching algorithm. The subexponential hidden-shift factor is doing real work and also marks the barrier — making the $2^{O(\sqrt{\log m})}$ term polylogarithmic would require progress on dihedral hidden shift.

## The algorithm / construction

### 1. Injectivise strings by local blocks

For a string $S:[n]^d\to\Sigma$, define $S^{\triangleright k}$ by replacing each position with the adjacent $k\times\cdots\times k$ block beginning there:

$$
S^{\triangleright k}(s)=S_{s,k}\in\Sigma^{k^d}.
$$

If $S^{\triangleright k}$ is injective, then matching $P$ inside $T$ becomes a hidden-shift-style problem between injective functions. For random strings, Lemma 6 shows that

$$
\Pr[\upsilon(S)\ge (3d\log_q n)^{1/d}]\le 1/n^d.
$$

So random strings become injective after only logarithmic-volume blocks.

### 2. Use hidden shift locally

The core subroutine takes injective functions

$$
f,g:\mathbb Z_{2^n}^d\to X
$$

with $g(x)\approx f(x+s)$ for some shift $s$, and recovers $s$ in

$$
O\!\left(n2^{\sqrt{(2\log_2 3)dn}}\right)
$$

queries to each function. This is a Kuperberg-type labelled-qubit sieve, modified in two useful ways:

1. It works over $\mathbb Z_{2^n}^d$ rather than just one cyclic factor.
2. Failed pair-combinations are recycled, improving the constant in the exponent.

The subroutine is close to [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg's dihedral hidden-shift sieve]], but the paper includes its own proof because the pattern-matching reduction needs an approximate version.

### 3. RoughCheck: test a coarse offset

Given a coarse offset $t$, the procedure `RoughCheck`:

1. Forms injectivised windows

   $$
   T'=(T^{\triangleright \nu})_{t,m'},\qquad P'=(P^{\triangleright \nu})_{0,m'},
   $$

   where $m'$ is the largest power of two below $m-\nu$.
2. Runs the hidden-shift algorithm on $T'$ and $P'$ to obtain an offset $\ell$.
3. Rejects if $\ell$ is not inside the intended tolerance window.
4. Otherwise checks whether $P$ matches $T$ at $t+\ell$ using amplitude amplification over mismatching coordinates, costing $O(1/\sqrt\gamma)$ queries.

### 4. Grover search over coarse offsets

If there is a true match, a random coarse offset lands near it with probability roughly $(\epsilon m/n)^d$. A bounded-error Grover search over coarse offsets therefore costs

$$
O\!\left((n/(\epsilon m))^{d/2}\right)
$$

calls to `RoughCheck`. Substituting the hidden-shift tolerance gives the theorem.

## Key results

**Theorem 1.** Let $m=\omega(\log n)$ and fix $d=O(1)$. Let $T:[n]^d\to\Sigma$ be uniformly random. Let $P:[m]^d\to\Sigma$ be either:

1. a substring of $T$, chosen from an arbitrary offset, or
2. an independent uniformly random pattern.

There is a quantum algorithm that distinguishes the two cases and, in case 1, outputs the match location in time

$$
\widetilde O\!\left((n/m)^{d/2}2^{O(d^{3/2}\sqrt{\log m})}\right).
$$

It fails with probability $O(1/n^d)$ over the random instance and internal randomness. Any classical bounded-error algorithm must make

$$
\widetilde\Omega\!\left(n^{d/2}+(n/m)^d\right)
$$

queries to $T$ and $P$.

**Theorem 2.** Under the structured promises $\upsilon(T,m),\upsilon(P)\le \nu\le m/2$ and every non-match being $\gamma$-far, there is a bounded-error quantum algorithm using

$$
O\!\left(\left(\frac{n\log^2 m\,2^{\sqrt{(2\log_2 3)d\log_2 m}}}{m}\right)^{d/2}
\left(\nu^d\log m\,2^{\sqrt{(2\log_2 3)d\log_2 m}}+\frac{1}{\sqrt\gamma}\right)\right)
$$

queries to each of $T$ and $P$.

**Theorem 4.** Approximate hidden shift over $\mathbb Z_{2^n}^d$: if $f,g$ are injective and

$$
\Pr_x[g(x)\ne f(x+s)] = O\!\left(n^{-2}2^{-\sqrt{(2\log_2 3)dn}}\right),
$$

then $s$ can be recovered with bounded error using

$$
O\!\left(n2^{\sqrt{(2\log_2 3)dn}}\right)
$$

queries to each function.

**Lemma 10.** Even under strong injectivity promises, pattern detection needs

$$
\Omega\!\left((n/m)^{d/2}/\sqrt\gamma\right)
$$

quantum queries and

$$
\Omega\!\left((n/m)^d/\gamma\right)
$$

randomised classical queries.

## Comparison with prior work

| Work | Setting | Complexity / role |
|---|---|---|
| Knuth-Morris-Pratt (1977) | Worst-case 1D classical matching | $\Theta(n+m)$ |
| Yao (1979) | Average-case classical lower bound | $\Omega((n/m)\log_q m)$ for 1D random strings |
| Kärkkäinen-Ukkonen (1999) | Average-case $d$D classical matching | $O((n/m)^d\log_q m + m^d)$ |
| Ramesh-Vinay (2003) | Worst-case 1D quantum string matching | $\widetilde O(\sqrt n)$ |
| [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]] | Hidden shift / dihedral HSP | $2^{O(\sqrt{\log N})}$ time and space |
| This paper | Average-case $d$D quantum pattern matching | $\widetilde O((n/m)^{d/2}2^{O(d^{3/2}\sqrt{\log m})})$ |

## Limits / caveats

- The advantage is average-case or promise-based. Worst-case verification of a claimed match can cost $\Omega(m^{d/2})$ quantum queries if mismatches may be sparse.
- The hidden-shift factor is subexponential in $\log m$, not polylogarithmic. It is asymptotically $m^{o(1)}$, but it is not small.
- The algorithm needs efficient oracle or qRAM-style access to $T$ and $P$; the statement assumes $O(1)$ query time.
- The approximate hidden-shift subroutine tolerates only very low error, roughly $2^{-O(\sqrt{\log m})}$ times polynomial factors.
- For small $m$, straightforward Grover/Ramesh-Vinay style methods can be better.

## Reusable ideas

1. [[Injectivise Pattern Matching by Adjacent Blocks]] — convert random or locally diverse strings into injective functions by treating short blocks as super-symbols.
2. [[Coarse Offset Search plus Hidden Shift Refinement]] — Grover-search rough locations, then use hidden shift to refine within a nearby window.
3. [[Kuperberg Sieve with Recycled Failures]] — reuse failed labelled-qubit combinations rather than discarding them, improving the sieve's constant.
4. [[Average-Case Lower Bounds via Collision Discovery]] — reduce random injective pattern detection to the need to find a collision between queried pattern and text symbols.
5. [[Low-Noise Approximate Hidden Shift by Trace-Distance Stability]] — keep an exact hidden-shift algorithm working under a very small mismatch rate by bounding the perturbation of its input mixed state.

## References within this paper

- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]] — hidden-shift sieve underlying the quantum subroutine.
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]] — polynomial-space variant, slower time.
- [[Quantum Algorithms for Some Hidden Shift Problems (van Dam-Hallgren-Ip 2002) — Paper Notes|van Dam-Hallgren-Ip (2006)]] — hidden-shift algorithms for special functions.
- [[Easy and Hard Functions for the Boolean Hidden Shift Problem (Childs-Kothari-Ozols-Roetteler 2013) — Paper Notes|Childs-Kothari-Ozols-Roetteler (2013)]] — Boolean hidden shift complexity landscape.
- [[Quantum Search on Bounded-Error Inputs (Høyer-Mosca-de Wolf 2003) — Paper Notes|Høyer-Mosca-de Wolf (2003)]] — bounded-error search wrapper used for `RoughCheck`.
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification for mismatch checking.
- Ramesh-Vinay (2003) — $\widetilde O(\sqrt n)$ quantum string matching in the worst case.
- Yao (1979) and Kärkkäinen-Ukkonen (1999) — classical average-case lower/upper bounds for random pattern matching.
- Montanaro-de Wolf (2013) — quantum property testing; cited for the promise-test perspective.

## Cross-links

### Paper notes

- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[Quantum Search with Advice (Montanaro 2009) — Paper Notes]] — another Montanaro paper exploiting distributional structure in search.
- [[Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes]] — another oracle-identification/search problem where query structure matters.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]

### Trick cards

- [[Injectivise Pattern Matching by Adjacent Blocks]]
- [[Coarse Offset Search plus Hidden Shift Refinement]]
- [[Kuperberg Sieve with Recycled Failures]]
- [[Average-Case Lower Bounds via Collision Discovery]]
- [[Standard Amplitude Amplification]]
- [[Quantum Sieve for Labelled Qubits]]
