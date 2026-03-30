> **Source:** Andris Ambainis, Andrew M. Childs, Yi-Kai Liu, *Quantum property testing for bounded-degree graphs*, RANDOM 2011, LNCS 6845, pp. 365–376; arXiv:1012.3174
> **Links:** [arXiv](https://arxiv.org/abs/1012.3174)
> **Tags:** #property-testing #quantum-walk #element-distinctness #graph-property #lower-bound #polynomial-method

---

## The computational problem

**Graph property testing** in the adjacency-list model: a bounded-degree graph $G = (V, E)$ with $N$ vertices and maximum degree $d$ is accessed via a black box $f_G(v, i)$ returning the $i$th neighbour of $v$ (or $*$ if $v$ has fewer than $i$ neighbours).

Given a property $P$, the tester must:
- Accept with probability $\geq 2/3$ if $G$ satisfies $P$
- Reject with probability $\geq 2/3$ if $G$ is $\varepsilon$-far from $P$ (i.e., $\geq \varepsilon d N$ edge modifications needed)

Two specific properties:
1. **Bipartiteness** — is the graph 2-colourable?
2. **$\alpha$-expansion** — does every subset $U$ with $|U| \leq N/2$ have $|\partial(U)| \geq \alpha |U|$?

Classical results (Goldreich-Ron): both can be tested in $\tilde{O}(\sqrt{N})$ queries, which is tight — $\Omega(\sqrt{N})$ lower bounds exist for both.

---

## What the paper does

Gives $\tilde{O}(N^{1/3})$ quantum algorithms for testing both bipartiteness and expansion of bounded-degree graphs — a polynomial speedup over the classical $\Omega(\sqrt{N})$ lower bound. Also proves an $\tilde{\Omega}(N^{1/4})$ quantum lower bound for expansion testing, ruling out exponential speedup.

The approach is clean and modular: take the existing classical property testers (which work by detecting collisions among random walk endpoints), derandomize them to use $O(\text{polylog } N)$ random bits, then plug in [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis's element distinctness algorithm]] to find collisions in $\tilde{O}(N^{1/3})$ time.

---

## The algorithm

### Bipartiteness testing

The Goldreich-Ron classical tester for bipartiteness works as follows:
1. Repeat $T = \Theta(1/\varepsilon)$ times:
   - Pick a random starting vertex $s$
   - Run $K = \sqrt{N} \cdot \text{polylog}$ random walks from $s$, each of length $L = \text{polylog}$
   - Look for "odd collisions": two walks reaching the same vertex $v$, one after an even number of steps, the other after odd. Such a collision certifies a short odd cycle → non-bipartiteness.

The quantum speedup comes from the collision-finding step. The $K$ random walks produce $K$ endpoint-parity pairs; detecting an odd collision among them is an instance of element distinctness.

### Derandomization

Each repetition uses $n = O(KL \log d)$ random bits. The analysis of the classical algorithm (Chebyshev on the collision count $X = \sum_{i<j} \eta_{ij}$) only requires $k$-wise independence for $k = O(L \log d)$ — because $\mathrm{E}[X]$ and $\mathrm{Var}[X]$ depend on correlations among at most $O(L \log d)$ bits. Substituting $k$-wise independent bits reduces the randomness to $O(k \log n) = O(\text{polylog } N)$ bits per repetition.

This matters because the [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|element distinctness algorithm]] needs the function to be explicitly computable — the random walk endpoints must be deterministic functions of an index, which requires the derandomized version.

### Plugging in element distinctness

With $K = \sqrt{N} \cdot \text{polylog}$ items and $O(\text{polylog})$ time per function evaluation, [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis's algorithm]] finds a collision (if one exists) in $\tilde{O}(K^{2/3}) = \tilde{O}(N^{1/3})$ time.

### Expansion testing

The approach is similar but slightly more involved:
1. Run $K = N^{1/2+\mu}$ random walks from a starting vertex, each of length $L = O((d^2/\alpha^2)\log N)$
2. Count collisions among endpoints — many collisions indicate the walk isn't mixing rapidly → poor expansion
3. The collision count threshold is $M \approx \frac{1}{2}N^{2\mu}$

Counting (not just detecting) collisions requires a wrapper: run the element distinctness algorithm $M$ times, each time excluding previously found collisions. This costs $O(M \cdot \tilde{O}(K^{2/3})) = O(N^{1/3 + 3\mu} \cdot \text{polylog})$.

---

## Key results

$$\text{Bipartiteness testing:} \quad \tilde{O}(N^{1/3}) \text{ quantum queries and time}$$

$$\text{Expansion testing:} \quad \tilde{O}(N^{1/3 + 3\mu}) \text{ time for parameter } \mu$$

$$\text{Expansion lower bound:} \quad \tilde{\Omega}(N^{1/4}) \text{ quantum queries}$$

| Problem | Classical | Quantum (this paper) | Quantum lower bound |
|---|---|---|---|
| Bipartiteness | $\tilde{\Theta}(\sqrt{N})$ | $\tilde{O}(N^{1/3})$ | open |
| Expansion | $\tilde{\Theta}(\sqrt{N})$ | $\tilde{O}(N^{1/3})$ | $\tilde{\Omega}(N^{1/4})$ |

---

## The quantum lower bound for expansion

This is the most technically involved part. The proof uses the polynomial method, adapted to handle the combinatorial structure of random graphs.

**Hard distribution:** Random graphs $G$ drawn from $P_{M,\ell}$ — construct $\ell$ random expander components on $M$ vertices, then sample an $N$-vertex induced subgraph. With $\ell = 1$, $G$ is an expander w.h.p. With $\ell = 2$, $G$ is far from any expander.

**Core argument:**
1. The acceptance probability of any $T$-query quantum algorithm on input from $P_{M,\ell}$ is well-approximated by a ratio $f(M,\ell)/g(M,\ell)$ where $f, g$ are polynomials of degree $O(T \log T)$
2. The key technical lemma analyses how monomials in the acceptance polynomial decompose over connected components of query graphs, using partition lattice combinatorics and inclusion-exclusion
3. A polynomial degree lower bound (following Aaronson's collision lower bound approach + Paturi's theorem on polynomial approximation) gives $\deg f = \Omega(\sqrt{\delta}) = \Omega(N^{1/4})$

The algebraic techniques for handling the graph structure — expressing probabilities over random matchings as rational functions of $M$ and $\ell$, the partition identity (Proposition 5), and the careful degree analysis — are the main technical contribution.

---

## Comparison with prior work

| Technique | Classical property testing | This paper |
|---|---|---|
| Random walks | $\tilde{O}(\sqrt{N})$ walks | Same walks, derandomized |
| Collision finding | Classical: $\tilde{O}(\sqrt{N})$ (birthday paradox) | Quantum: $\tilde{O}(N^{1/3})$ via [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes\|element distinctness]] |
| Lower bound technique | Classical adversary | Polynomial method with graph-structured inputs |

Prior quantum property testing results achieved exponential speedups for some problems (e.g., testing properties defined by the Fourier transform), but those problems have more algebraic structure. For graph properties, this paper establishes that polynomial (not exponential) quantum speedups are the norm — at least for expansion.

---

## Limits / caveats

- The bipartiteness lower bound is **open**. The paper cannot prove any superconstant quantum lower bound for bipartiteness testing. Whether there's a bigger gap between the $\tilde{O}(N^{1/3})$ upper bound and some (unknown) lower bound remains unresolved.
- The expansion algorithm's exponent $1/3 + 3\mu$ is not tight for all parameter regimes — there's a gap between the $\tilde{O}(N^{1/3})$ upper bound and $\tilde{\Omega}(N^{1/4})$ lower bound.
- The approach is specific to bounded-degree graphs in the adjacency-list model. Dense graphs (adjacency-matrix model) are a different story.
- The derandomization step is critical for the quantum speedup but limits the approach to algorithms whose analysis survives $k$-wise independence. Not all classical property testers have this property.

---

## Reusable ideas

1. [[Derandomize-then-Quantize for Property Testing]] — Derandomize a classical property tester to use polylog random bits (via $k$-wise independence), then apply a quantum collision/search algorithm. This template converts classical collision-based testers into quantum algorithms with polynomial speedup.

2. [[Polynomial Method for Graph-Structured Inputs]] — Adapt the polynomial method to inputs with graph structure (random matchings, partition lattice combinatorics) to prove quantum query lower bounds for property testing. The technical contribution is expressing acceptance probabilities as rational functions of graph parameters and bounding their degree.

---

## References within this paper

- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — the element distinctness algorithm that provides the collision-finding subroutine
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|Magniez-Nayak-Roland-Santha (2007)]] — the unified quantum walk search framework; related but not directly used
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — quantum walk formalism that underlies the element distinctness approach
- Goldreich-Ron (2002) — classical bipartiteness testing algorithm; the starting point for the quantum version
- Goldreich-Ron (2000), Kale-Seshadhri (2008), Nachmias-Shapira (2010) — classical expansion testing algorithms
- Aaronson (2002) — collision lower bound; the polynomial method approach adapted here for graphs
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett-Bernstein-Brassard-Vazirani (1997)]] — $\Omega(\sqrt{N})$ quantum search lower bound; contrasted with the richer query model used in property testing
- [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes|Bravyi-Harrow-Hassidim (2011)]] — concurrent work on quantum distribution property testing

---

## Cross-links

### Paper notes
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes]]
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]]
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]]

### Trick cards
- [[Derandomize-then-Quantize for Property Testing]]
- [[Polynomial Method for Graph-Structured Inputs]]
- [[Coined Quantum Walk Search on Graphs]]
- [[Amplitude Estimation for Distribution Property Testing]]
