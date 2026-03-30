> **Source:** Scott Aaronson, Andris Ambainis, *Forrelation: A Problem That Optimally Separates Quantum from Classical Computing*, Proceedings of STOC 2015, pp. 307–316.
> **Links:** [arXiv:1411.5729](https://arxiv.org/abs/1411.5729) · [ACM](https://doi.org/10.1145/2746539.2746547)
> **Tags:** #query-complexity #quantum-classical-separation #lower-bound #BQP #PH #Fourier-analysis #boolean-functions

---

## The computational problem

**Forrelation (2-fold):** Given oracle access to two Boolean functions $f, g : \{0,1\}^n \to \{-1,+1\}$, compute the *forrelation value*

$$\Phi_{f,g} := \frac{1}{2^{3n/2}} \sum_{x, y \in \{0,1\}^n} f(x)\,(-1)^{x \cdot y}\,g(y) = \frac{1}{\sqrt{N}} \sum_{y} \hat{f}(y)\,g(y)$$

where $\hat{f}$ is the Boolean Fourier transform of $f$ and $N = 2^n$.

**Promise:** Either $|\Phi_{f,g}| \leq 1/100$ (unforrelated) or $\Phi_{f,g} \geq 3/5$ (forrelated). Decide which.

**$k$-fold Forrelation:** A generalisation with $k$ functions $f_1, \ldots, f_k : \{0,1\}^n \to \{-1,+1\}$:
$$\Phi_{f_1,\ldots,f_k} := \frac{1}{2^{(k+1)n/2}} \sum_{x_1,\ldots,x_k \in \{0,1\}^n} f_1(x_1)\,(-1)^{x_1 \cdot x_2}\,f_2(x_2)\,(-1)^{x_2 \cdot x_3} \cdots (-1)^{x_{k-1} \cdot x_k}\,f_k(x_k)$$

Same promise structure. The oracle has access to all $k$ functions; the total input size is $kN$ bits.

---

## What the paper does

Three results in one go:

1. **Optimal quantum-classical separation via a natural problem.** Forrelation (2-fold) has quantum query complexity $1$ but randomised query complexity $\tilde{\Omega}(\sqrt{N})$. This matches the general simulation upper bound (Theorem 3 below), so no separation wider than this 1-versus-$\tilde{\Omega}(\sqrt{N})$ gap is possible for any partial Boolean function.

2. **General simulation theorem.** Any $t$-query quantum algorithm on $N$ input bits can be simulated by a classical randomised algorithm making $O(N^{1-1/(2t)})$ non-adaptive queries. This resolves a 2002 open question of Buhrman et al.: there is no partial Boolean function with constant quantum query complexity and *linear* randomised query complexity.

3. **$k$-fold Forrelation is BQP-complete** (for $k = \text{poly}(n)$). This gives a concrete natural problem complete for BQP under classical reductions, analogous to what CIRCUIT-SAT is for NP.

My take: result 1 is the headline and it's genuinely clean. The separation was previously known for *total* Boolean functions (Grover: 1 vs. $\Omega(N^{1/2})$ for search) but for *partial* functions this is the first example achieving the optimal gap, and Forrelation is arguably the most natural possible problem for doing so — it's literally "how correlated is $f$ with the Fourier transform of $g$?" Result 2 is also important: it closes off a whole class of hoped-for separations.

---

## The quantum algorithm

**Direct circuit for 2-fold Forrelation.** A simple circuit uses one phase query to $f$ and one to $g$:

1. Prepare $|0^n\rangle$.
2. Apply $H^{\otimes n}$.
3. Apply the phase oracle $O_f$: $|x\rangle \mapsto f(x)|x\rangle$.
4. Apply $H^{\otimes n}$.
5. Apply the phase oracle $O_g$: $|y\rangle \mapsto g(y)|y\rangle$.
6. Apply $H^{\otimes n}$.
7. Measure in the computational basis.

In this circuit, the amplitude of $|0^n\rangle$ is exactly $\Phi_{f,g}$, so the probability of observing $|0^n\rangle$ is $\Phi_{f,g}^2$. Under the promise used in the paper, this is still enough to distinguish the two cases with constant bias: the YES case has probability at least $(3/5)^2 = 9/25$, while the NO case has probability at most $10^{-4}$.

**About the query count.** The paper states the 2-fold problem as a 1-query quantum problem in its oracle convention. The circuit above is the clearest way to see the Fourier-correlation structure, but operationally it is best thought of as a constant-query algorithm with one phase query to each function. I am being slightly more conservative here than the paper's headline statement because the exact "1 query" accounting depends on how the joint oracle for $(f,g)$ is packaged.

**Improved: $\lceil k/2 \rceil$ queries for $k$-fold Forrelation.** The paper also gives a cleaner query-count construction for $k$-fold Forrelation: prepare a control qubit in $|+\rangle$, run the first half of the chain on one branch and the second half backwards on the other, then interfere the two branches. This reduces the naive $k$-query implementation to $\lceil k/2 \rceil$ queries.

---

## Randomised lower bound for Forrelation

**Main theorem (Theorem 1):** Any bounded-error classical randomised algorithm deciding Forrelation requires $\tilde{\Omega}(\sqrt{N})$ queries to the combined oracle $(f, g)$.

### The proof strategy: Gaussian reduction

The key move is to lift from discrete $\pm 1$ Boolean functions to continuous Gaussian functions, prove the lower bound in the Gaussian setting, then transfer back. This two-step approach is clean and the Gaussian version is easier to analyse because the underlying linear algebra is standard.

**Step 1 — Real Forrelation:** Replace $f, g : \{0,1\}^n \to \{-1,+1\}$ with $f, g : \{0,1\}^n \to \mathbb{R}$ and define $\Phi_{f,g}$ identically. The promise becomes: either $f$ and $g$ are drawn i.i.d. from $\mathcal{N}(0,1)$ per coordinate (null hypothesis $H_0$), or $f \sim \mathcal{N}(0,I_N)$ and $g = Hf$ where $H$ is the normalised $N \times N$ Hadamard matrix (alternative $H_1$). Under $H_1$, $g$ is also marginally $\mathcal{N}(0,I_N)$ but $f$ and $g$ are correlated.

**Step 2 — Transfer from Gaussian to Boolean:** Round each Gaussian coordinate to $\pm 1$ by sign. Show (Theorem 9) that this rounding preserves the promise gap with high probability — the forrelation value concentrates well enough that Boolean-hard instances are produced by rounding Gaussian-hard instances.

### Gaussian Distinguishing lower bound

The core technical lemma:

**Theorem (Gaussian Distinguishing):** Suppose $v \in \mathbb{R}^M$ is drawn from either $H_0$ (all $M$ coordinates i.i.d. $\mathcal{N}(0,1)$) or $H_1$ (a Gaussian on an $N$-dimensional linear subspace, so the coordinates can covary, but each pair of coordinates has covariance $|\text{Cov}(v_i, v_j)| \leq \varepsilon$). Any randomised algorithm that distinguishes $H_0$ from $H_1$ with constant bias must make $\tilde{\Omega}(1/\varepsilon)$ queries (up to log factors in $M/\varepsilon$).

For Forrelation, $M = 2N$ (both $f$ and $g$ coordinates), $\varepsilon = 1/\sqrt{N}$, so the bound gives $\tilde{\Omega}(\sqrt{N})$.

### Proof of Gaussian Distinguishing

This is the technical heart of the paper. The argument is a martingale / orthogonality analysis.

**Setup:** Model an adaptive algorithm as revealing entries of $v$ one at a time. After $t$ queries, the algorithm has observed a set $S$ of entries.

**Bayesian posterior:** Under $H_0$, the unobserved entries are still i.i.d. $\mathcal{N}(0,1)$. Under $H_1$, they are Gaussian with posterior covariance determined by projecting off the observed entries. Define:
- $\Delta_t^{(0)}$: the squared norm the algorithm expects the remaining unobserved subvector to have under $H_0$ (equals $M - t$ always).
- $\Delta_t^{(1)}$: the corresponding quantity under $H_1$.

Distinguishing succeeds only if $|\Delta_t^{(0)} - \Delta_t^{(1)}|$ becomes large.

**Gram–Schmidt lemma:** The $2N$ vectors in $V = \{e_x\}_{x \in \{0,1\}^n} \cup \{Hx\}_{x \in \{0,1\}^n}$ (standard basis vectors and their Hadamard images) have pairwise inner products $\leq 1/\sqrt{N}$. This near-orthogonality means that revealing one coordinate gives very little information about the others under $H_1$.

**Martingale bound:** Each query contributes at most $O(1/\sqrt{N})$ to $|\Delta_t^{(0)} - \Delta_t^{(1)}|$. Apply Azuma's inequality: after $T$ queries, $|\Delta_T^{(0)} - \Delta_T^{(1)}| \leq O(T/\sqrt{N}) + O(\sqrt{T/\sqrt{N}})$ with high probability. For this to be $\Omega(1)$ we need $T = \tilde{\Omega}(\sqrt{N})$.

The recursive improvement (from the initial $\Omega(N^{1/4})$ bound to $\tilde{\Omega}(\sqrt{N})$) is achieved by two nested applications of Azuma, exploiting that the total number of vectors in $V$ is only $2N$, so individual coordinate deviations are tightly controlled.

---

## General simulation theorem

**Theorem 3:** Any $t$-query quantum algorithm on $N$ input bits can be simulated by a classical randomised algorithm that non-adaptively queries $O(N^{1-1/(2t)})$ input bits (and runs in $\text{poly}(N)$ time).

### The construction

The simulation works via **block-multilinear polynomial approximation**:

1. **Represent quantum acceptance probability as a polynomial.** The acceptance probability of a $t$-query quantum algorithm on a Boolean input $x \in \{0,1\}^N$ is a multilinear polynomial in $x$ of degree at most $2t$ (a standard fact). For $\pm 1$-valued inputs, this becomes a real polynomial of degree $\leq 2t$.

2. **Estimate via Fourier sampling.** A degree-$2t$ multilinear polynomial over $N$ variables can be estimated from $O(N^{1-1/(2t)})$ random samples. The key is that the polynomial can be decomposed into blocks corresponding to Fourier coefficients, and the contribution from high-degree terms averages to zero under random sampling.

3. **Block-multilinear decomposition.** Write $p(x) = \sum_S \hat{p}(S) \prod_{i \in S} x_i$. The algorithm: sample $m = O(N^{1-1/(2t)})$ coordinates uniformly at random, query them, and estimate $p(x)$ via an unbiased estimator derived from the sampled entries. The estimator's variance is controlled by the $\ell_2^2$ norm of the Fourier coefficients, which is bounded by the acceptance probability squared.

**Corollary:** There is no partial Boolean function with quantum query complexity $O(1)$ and randomised query complexity $\Omega(N)$. Any $t$-vs-$T$ separation must satisfy $T = O(N^{1-1/(2t)})$, so Forrelation's $1$-vs-$\tilde{\Omega}(\sqrt{N})$ gap is optimal.

---

## Key results

| Result | Statement |
|---|---|
| Quantum query complexity of Forrelation | $Q(F) = 1$ in the paper's oracle convention (equivalently: constant quantum query complexity) |
| Randomised query complexity of Forrelation | $R(F) = \tilde{\Omega}(\sqrt{N})$ |
| Simulation theorem | $t$ quantum queries $\Rightarrow$ $O(N^{1-1/(2t)})$ classical queries |
| Separation optimality | No partial Boolean function has $Q = O(1)$, $R = \Omega(N)$ |
| $k$-fold Forrelation (k=poly(n)) | BQP-complete |

**Theorem 1 (precise bound):** The randomised query complexity of Forrelation is $\Omega\!\left(\frac{\sqrt{N}}{\log N}\right)$.

**Theorem 2 (simulation):** Any quantum algorithm making $t$ queries to $N$-bit input can be simulated classically using $\binom{N}{\leq 2t}^{1/2} = O(N^{1-1/(2t)})$ queries (with appropriate constants depending on $t$).

---

## Comparison with prior work

| Paper | Result | What they showed |
|---|---|---|
| [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes\|Deutsch-Jozsa (1992)]] | 1-query QC, $\Omega(N/2)$ deterministic | Exponential separation vs. *deterministic* |
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes\|Simon (1994)]] | $O(n)$ QC, exponential randomised | Exponential separation vs. randomised, but *promise-hiding* structure |
| [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes\|BBBV (1997)]] | [[Query Magnitude Hybrid Argument for Oracle Lower Bounds\|hybrid argument]] | Grover is optimal for unstructured search ($\Omega(\sqrt{N})$ lower bound) |
| Aaronson (2010) | $\Omega(N^{1/4})$ randomised lower bound for Forrelation | First lower bound for this specific problem |
| Buhrman et al. (2002) | Open: is there $Q=O(1)$, $R=\Omega(N)$? | Conjectured no |
| **This paper (2015)** | $Q=1$, $R=\tilde{\Omega}(\sqrt{N})$, simulation theorem | **Resolves Buhrman et al.; proves this gap is optimal** |

---

## Limits and caveats

- The $\tilde{\Omega}(\sqrt{N})$ bound has a $\log N$ gap to the simulation upper bound $O(\sqrt{N})$. Whether Forrelation requires exactly $\Theta(\sqrt{N})$ or $\Theta(\sqrt{N}/\log N)$ queries is not fully settled.
- The simulation theorem applies to *query* complexity, not circuit complexity. A constant-query quantum algorithm may still have exponential circuit complexity; the simulation says nothing about time complexity.
- Forrelation is a *partial* Boolean function — the promise is non-trivial. The separation disappears if you require the function to be total (BBBV shows any total Boolean function has $R \leq Q^6$ roughly, so no super-polynomial separation is possible classically vs. randomly for total functions).
- The BQP-completeness result requires $k = \text{poly}(n)$, which means the input size scales with $k$ as well. For fixed small $k$, the problem is not known to be BQP-complete.

---

## Reusable ideas

1. **Gaussian lifting for Boolean query lower bounds.** Replace $\pm 1$ Boolean functions with Gaussian inputs, prove the lower bound in continuous Gaussian settings (where Fourier analysis is linear algebra), then transfer back via rounding. See [[Gaussian Lifting for Query Lower Bounds]].

2. **Near-orthogonality of standard-Fourier basis pairs.** The set $V$ of $N$ standard basis vectors and their $N$ Hadamard images has all pairwise inner products bounded by $1/\sqrt{N}$. This controls information leakage per query. See [[Near-Orthogonality of Standard-Fourier Basis Pairs]].

3. **Block-multilinear polynomial simulation of quantum queries.** The acceptance probability of any $t$-query quantum algorithm is a degree-$2t$ multilinear polynomial; this polynomial can be estimated from $O(N^{1-1/(2t)})$ random samples, giving a general classical simulation. See [[Block-Multilinear Polynomial Simulation of Quantum Queries]].

---

## References within this paper

- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|BBBV (1997)]] — [[Query Magnitude Hybrid Argument for Oracle Lower Bounds|hybrid argument]], Grover lower bound
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani (1993)]] — BQP definition, first quantum-vs-randomised separation
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — Simon's problem, exponential quantum speedup
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes|Deutsch-Jozsa (1992)]] — exact query separation vs. deterministic
- [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes|Aaronson (2010)]], *BQP and the polynomial hierarchy* — introduced Forrelation, proved $\Omega(N^{1/4})$ lower bound; also showed $k$-fold Forrelation $\in$ PH (via approximate Fourier sampling); first evidence BQP $\not\subseteq$ PH
- Buhrman, Cleve, de Wolf, Zalka (2002) — open question on whether $Q=O(1), R=\Omega(N)$ is achievable
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — quantum walk approach to query complexity, separations
- Nisan and Szegedy (1994) — polynomial method for query complexity lower bounds (influential technique for this line of work)
- Beigel (1993), Aspnes, Beigel, Furst, Rudich (1994) — classical results on Boolean functions and Fourier analysis used in the simulation proof

---

## Cross-links

### Paper notes
- [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes]] — predecessor paper introducing Forrelation and Fourier Checking
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]] — the polynomial method; classical simulation theorem in Forrelation paper extends ideas from this line
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]] — Ambainis is a coauthor; adversary method provides complementary lower bound techniques
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — source of the hybrid argument and Grover lower bound
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]] — BQP definition
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — exponential quantum speedup
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]] — earlier exact-query separation
- [[Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) — Paper Notes]] — earlier 1-vs-$\Omega(2^{n/2})$ exact quantum separation over finite fields; this paper's gap matches it but for a more natural problem and with the optimal simulation theorem
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — the $O(\sqrt{N})$ upper bound that matches this lower bound for total functions
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — query complexity separations via walk methods
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — related quantum-classical separation story via sampling
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — earlier Aaronson paper; block-multilinear polynomial idea recurs there
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]] — BQP-completeness result for coupled oscillator simulation; the glued-trees oracle separation underlying that result is in the same spirit as Forrelation's optimal query separation
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]] — quartic query separation for planted $k$XOR; different oracle model but same project of finding natural problems with provable quantum-classical gaps

### Trick cards
- [[Gaussian Lifting for Query Lower Bounds]]
- [[Near-Orthogonality of Standard-Fourier Basis Pairs]]
- [[Block-Multilinear Polynomial Simulation of Quantum Queries]]
- [[Query Magnitude Hybrid Argument for Oracle Lower Bounds]]
