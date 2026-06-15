# Optimal Ancilla-Free Clifford+T Approximation of Z-Rotations (Ross-Selinger 2014)

> **Source:** Neil J. Ross and Peter Selinger, *Optimal ancilla-free Clifford+T approximation of z-rotations*, arXiv:1403.2975 (2014; v3 2016)
> **Links:** [arXiv](https://arxiv.org/abs/1403.2975) · [PDF](https://arxiv.org/pdf/1403.2975)
> **Tags:** #gate-synthesis #Clifford-T #z-rotations #number-theory #fault-tolerance

---

## The computational problem

Given an angle $\theta$ and precision $\varepsilon>0$, synthesize a single-qubit [[Clifford Hierarchy for Fault-Tolerant Gate Classification|Clifford+T]] circuit $U$ approximating

$$
R_z(\theta)=e^{-i\theta Z/2}=\begin{pmatrix}e^{-i\theta/2}&0\\0&e^{i\theta/2}\end{pmatrix}
$$

in operator norm,

$$
\lVert R_z(\theta)-U\rVert \le \varepsilon,
$$

with minimum possible T-count and no ancilla qubits. The paper also solves the usual physical variant up to global phase: find $U$ and a unit scalar $\lambda$ with

$$
\lVert R_z(\theta)-\lambda U\rVert \le \varepsilon .
$$

The cost measure is T-count, with Clifford gates treated as cheap.

**Model box:** deterministic ancilla-free Clifford+T synthesis; target is an $R_z$ rotation; error is operator norm; global phase is either fixed or optimized over in the explicit up-to-phase variant. These assumptions are what make the optimality claim precise.

---

## What the paper does

Ross and Selinger give a probabilistic number-theoretic synthesis algorithm for $R_z(\theta)$ over [[Clifford Hierarchy for Fault-Tolerant Gate Classification|Clifford+T]]. With an integer factoring oracle it is exactly T-count optimal for each input instance, not merely asymptotically optimal. Without factoring it is still near-optimal under a prime-distribution hypothesis and in the typical case produces T-count

$$
3\log_2(1/\varepsilon)+O(\log\log(1/\varepsilon)).
$$

The real technical move is turning candidate selection into a two-dimensional grid problem in $\mathbb D[\omega]$ and solving that grid problem fast by applying special grid operators until the geometry is easy.

---

## The algorithm / construction

The exact-representability theorem of Kliuchnikov--Maslov--Mosca says that a single-qubit unitary is exactly [[Clifford Hierarchy for Fault-Tolerant Gate Classification|Clifford+T]] iff its entries lie in

$$
\mathbb D[\omega]=\mathbb Z[1/\sqrt 2,i],\qquad \omega=e^{i\pi/4}.
$$

So the synthesis target is a unitary of the form

$$
U=\begin{pmatrix}u&-t^\dagger\\t&u^\dagger\end{pmatrix},\qquad u,t\in \mathbb D[\omega],
$$

where unitarity is exactly

$$
u^\dagger u+t^\dagger t=1.
$$

For $\varepsilon<|1-e^{i\pi/8}|$, all relevant solutions can be put in this form. If $\varepsilon$ is larger, a Clifford solution already suffices.

The construction has three layers.

### 1. Convert approximation into a region for $u$

Let $z=e^{-i\theta/2}$. Using $u^\dagger u+t^\dagger t=1$,

$$
\lVert R_z(\theta)-U\rVert^2=2-2\operatorname{Re}(z^\dagger u).
$$

Thus $U$ is an $\varepsilon$-approximation iff

$$
\operatorname{Re}(z^\dagger u)\ge 1-\frac{\varepsilon^2}{2}.
$$

Geometrically, $u$ must lie in the cap

$$
R_\varepsilon=\left\{\vec u\in D: \vec u\cdot \vec z\ge 1-\frac{\varepsilon^2}{2}\right\}
$$

inside the unit disk $D$. A necessary condition from unitarity is also $u^\bullet\in D$, where $(-)^\bullet$ is the $\sqrt 2$-conjugation automorphism sending $\sqrt 2\mapsto -\sqrt 2$.

So candidate search becomes:

$$
u\in R_\varepsilon,
\qquad
u^\bullet\in D,
\qquad
u\in\mathbb D[\omega].
$$

### 2. Enumerate candidates by denominator exponent

For $k\ge 0$, the scaled grid problem asks for

$$
u\in \frac{1}{\sqrt{2}^{\,k}}\mathbb Z[\omega],
\qquad
u\in A,
\qquad
u^\bullet\in B.
$$

Here $A=R_\varepsilon$ and $B=D$. The least denominator exponent $k$ controls T-count: if $U$ is of the above form and $k$ is the least denominator exponent of $u$, then the T-count is either $2k-2$ or $2k$; conjugating by $T$ gives a circuit of T-count $2k-2$ when $k>0$.

The paper's grid solver works as follows.

1. Enclose the convex sets $A,B\subset\mathbb R^2$ in ellipses with only constant-factor area loss.
2. Apply special grid operators $G$ with compatible action on $u$ and $u^\bullet$, preserving the solution set via
   $$
   u\mapsto Gu,
   \qquad
   u^\bullet\mapsto G^\bullet u^\bullet.
   $$
3. Iterate the Step Lemma from Appendix A to reduce the skew of the enclosing ellipses until both transformed regions are sufficiently upright.
4. Once the regions are upright, reduce the two-dimensional enumeration to one-dimensional grid enumeration in $\mathbb Z[\sqrt 2]$.
5. Enumerate candidates in increasing $k$.

This is the reusable geometric part of the paper: solve algebraic-integer approximation by normalising the convex geometry rather than searching a huge lattice box directly.

### 3. Solve the norm equation for $t$

For each candidate $u$, set

$$
\xi=1-u^\dagger u\in\mathbb D[\sqrt 2].
$$

The remaining task is the norm equation

$$
t^\dagger t=\xi.
$$

The paper reduces this to integer factorization: write

$$
\xi^\bullet\xi=\frac{n}{2^\ell}
$$

with $n\in\mathbb Z$ and $\ell$ minimal. Given the prime factorization of $n$, a probabilistic algebraic-number-theoretic algorithm decides whether $t$ exists and constructs it when it does. This is the only point where the factoring oracle enters.

### Up-to-phase version

For global phase, it is enough to test only two phases:

$$
\lambda\in\{1,e^{i\pi/8}\}.
$$

The $\lambda=1$ branch is the algorithm above. The $\lambda=e^{i\pi/8}$ branch rescales by $\delta=1+\omega$ and solves the analogous grid problem

$$
u'\in |\delta|R_\varepsilon,
\qquad
(\nu')^\bullet\in |\delta^\bullet|D,
$$

then runs the same norm-equation step. The final algorithm runs both branches and returns the smaller T-count.

---

## Key results

### Exact optimality with factoring

With an integer factoring oracle, Algorithm 7.6 returns a Clifford+T circuit of minimum T-count among all ancilla-free circuits satisfying

$$
\lVert R_z(\theta)-U\rVert\le \varepsilon.
$$

For the up-to-phase problem, Algorithm 9.9 is also exactly T-count optimal with a factoring oracle.

### Near-optimality without factoring

Assuming the paper's Hypothesis 8.3 — that the integers $n$ generated by the norm-equation step behave like independent random odd integers of comparable size for primality — if $m$ is the T-count returned by Algorithm 7.6 without a factoring oracle, then:

$$
\mathbb E[m]=m''+O(\log\log(1/\varepsilon)),
$$

where $m''$ is the T-count of the second-to-optimal non-equivalent solution.

For the up-to-phase version, the corresponding statement is

$$
\mathbb E[m]=m'''+O(\log\log(1/\varepsilon)),
$$

where $m'''$ is the third-to-optimal solution up to equivalence.

### Typical T-count

Under the same prime-distribution hypothesis / typical-case model used in the near-optimality theorem, the returned T-count is

$$
3\log_2(1/\varepsilon)+O(\log\log(1/\varepsilon)).
$$

This matches the information-theoretic lower-bound constant for ancilla-free $R_z$ synthesis.

### Worst-case T-count

The algorithm always does at least as well as Selinger's earlier ancilla-free method, giving worst-case T-count

$$
K+4\log_2(1/\varepsilon),
$$

for a constant $K\approx 10$.

The paper conjectures a sharp angle-dependent split:

$$
\tan(\theta/2)\in\mathbb Q(\sqrt 2)
\Rightarrow K+4\log_2(1/\varepsilon),
$$

whereas generically

$$
3\log_2(1/\varepsilon)+O(1)
$$

is expected.

### Runtime

Both with and without a factoring oracle, the expected running time is

$$
O(\operatorname{polylog}(1/\varepsilon)).
$$

The proof uses: grid enumeration cost $O(\log(1/M))$ plus constant arithmetic per candidate, uprightness $M=\Omega(\varepsilon^4)$ for the cap $R_\varepsilon$, expected $O(\log(1/\varepsilon))$ candidates, and candidate denominator exponents $k=O(\log(1/\varepsilon))$.

---

## Comparison with prior work

| Method | Ancillae | T-count / size | Runtime | Assessment |
|---|---:|---:|---:|---|
| [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Solovay--Kitaev]] | no | $O(\log^c(1/\varepsilon))$, $c>3$ | polylog | General but far from the right constant for single-qubit synthesis. |
| Exhaustive search / Fowler-style search | no | optimal for searched range | exponential | Finds best circuits but only at modest precisions. |
| Kliuchnikov--Maslov--Mosca 2013 | yes, constant | asymptotically $O(\log(1/\varepsilon))$ | polynomial | Allows ancillae, so it is not the same optimisation problem. |
| Selinger 2015 | no | $K+4\log_2(1/\varepsilon)$ | polylog | The direct predecessor; fast but not optimal in the typical constant. |
| Ross--Selinger 2014 | no | optimal with factoring; typically $3\log_2(1/\varepsilon)+O(\log\log(1/\varepsilon))$ without factoring under the prime-distribution hypothesis | expected $O(\operatorname{polylog}(1/\varepsilon))$ | The right ancilla-free result for $R_z$ rotations. |
| [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes|Wiebe--Kliuchnikov 2013]] | yes | can beat ancilla-free bounds for small rotations | expected | Different model: uses measurements/ancillae and [[Repeat-Until-Success with Clifford Correction|repeat-until-success]] ideas. |

---

## Limits / caveats

- **Only $R_z$ rotations.** The method does not directly solve optimal synthesis for arbitrary single-qubit unitaries. Euler-angle decomposition gives arbitrary single-qubit synthesis using three $z$-rotations, but the T-count becomes $9\log_2(1/\varepsilon)+O(\log\log(1/\varepsilon))$, while the information-theoretic lower bound for arbitrary single-qubit unitaries is $K+3\log_2(1/\varepsilon)$.
- **Exact optimality uses factoring.** A quantum computer can supply the factoring oracle via [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor]], but as a classical compilation algorithm the exact-optimal guarantee depends on factoring potentially large integers.
- **The no-oracle near-optimal proof is heuristic at one point.** Hypothesis 8.3 is a plausible prime-distribution assumption, not a theorem.
- **Worst-case angles remain worse by constant slope.** When $\tan(\theta/2)\in\mathbb Q(\sqrt 2)$, the grid geometry aligns badly with the thin cap and the expected scaling is $4\log_2(1/\varepsilon)$ rather than $3\log_2(1/\varepsilon)$.
- **Ancilla-free is a modelling choice.** For fault-tolerant compilation this is often the right primitive, but ancilla-assisted or RUS methods can beat ancilla-free T-count bounds in other regimes.

---

## Reusable ideas

1. [[Convex Grid Normalisation for Algebraic-Integer Candidate Search]] — turn algebraic-integer search with conjugation constraints into a convex grid problem, then use grid operators to make the geometry easy before enumeration.
2. [[Norm-Equation Completion of Clifford+T Matrix Entries]] — pick the top-left entry $u$ first, then complete unitarity by solving $t^\dagger t=1-u^\dagger u$ using algebraic number theory.
3. [[Two-Phase Reduction for Global-Phase Clifford+T Synthesis]] — reduce arbitrary global phase in Clifford+T approximation to the two cases $\lambda=1$ and $\lambda=e^{i\pi/8}$.

---

## References within this paper

- Bocharov, Gurevich, and Svore (2013) — Clifford+V single-qubit decomposition; cited as a parallel number-theoretic synthesis direction.
- Bocharov, Roetteler, and Svore (2015) — probabilistic quantum circuits with fallback and universal RUS circuits; relevant to the ancilla-assisted synthesis landscape.
- Fowler (2011) — exhaustive / search-based fault-tolerant gate construction baseline.
- Giles and Selinger (2013) — exact multiqubit Clifford+T synthesis and single-qubit normal forms; used for denominator-exponent/T-count facts.
- Kliuchnikov, Bocharov, and Svore (2014) — asymptotically optimal topological compiling for $\langle F,T\rangle$.
- Kliuchnikov, Maslov, and Mosca (2013) — exact Clifford+T synthesis and ancilla-assisted approximate synthesis; supplies the algebraic exact-representability backbone.
- Rabin (1980) — probabilistic finite-field algorithms used in the number-theoretic norm-equation routines.
- Selinger (2015) — earlier fast ancilla-free Clifford+T approximation with $K+4\log_2(1/\varepsilon)$ T-count.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — factoring oracle needed for exact optimality.
- [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes|Wiebe--Kliuchnikov (2013)]] — floating-point synthesis and related number-theoretic compilation context.

---

## Cross-links

### Paper notes

- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]]
- [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes]]
- [[Toward the First Quantum Simulation with Quantum Speedup (Childs-Maslov-Nam-Ross-Su 2018) — Paper Notes]]

### Trick cards

- [[Convex Grid Normalisation for Algebraic-Integer Candidate Search]]
- [[Norm-Equation Completion of Clifford+T Matrix Entries]]
- [[Two-Phase Reduction for Global-Phase Clifford+T Synthesis]]
- [[Clifford Hierarchy for Fault-Tolerant Gate Classification]]
- [[Repeat-Until-Success with Clifford Correction]]
