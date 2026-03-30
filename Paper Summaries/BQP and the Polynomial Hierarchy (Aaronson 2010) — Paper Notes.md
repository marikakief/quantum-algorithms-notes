> **Source:** Scott Aaronson, *BQP and the polynomial hierarchy*, Proceedings of STOC 2010, pp. 141–150; arXiv:0910.4698
> **Links:** [arXiv](https://arxiv.org/abs/0910.4698) · [ACM](https://doi.org/10.1145/1806689.1806711)
> **Tags:** #BQP #PH #oracle-separation #Forrelation #Fourier-analysis #circuit-complexity #AC0 #query-complexity

---

## The computational problem

**Fourier Checking:** Given oracle access to two Boolean functions $f, g : \{0,1\}^n \to \{-1,+1\}$, promised that either:
- (i) $\langle f, g \rangle$ drawn from the uniform distribution $U$ (all values independent fair coins), or
- (ii) $\langle f, g \rangle$ drawn from the "forrelated" distribution $F$: choose a random Gaussian vector $v$, set $f(x) = \text{sgn}(v_x)$ and $g(y) = \text{sgn}(\hat{v}_y)$ where $\hat{v}$ is the Fourier transform of $v$.

Decide which case holds.

**Fourier Fishing (relation problem):** Given oracle access to $n$ random Boolean functions $f_1, \ldots, f_n$, output strings $z_1, \ldots, z_n$ such that "many" of the corresponding Fourier coefficients $\hat{f}_i(z_i)$ are large (at least 75% have $|\hat{f}_i(z_i)| \geq 1$, at least 25% have $|\hat{f}_i(z_i)| \geq 2$).

---

## What the paper does

This is the paper that introduced Forrelation and gave the first formal evidence that BQP $\not\subseteq$ PH. Three main results:

1. **Relation problem separation (unconditional):** There exists an oracle $A$ relative to which FBQP$^A \not\subseteq$ FBPP$^{\text{PH}^A}$ — a BQP relation problem that the entire polynomial hierarchy can't solve. The underlying problem is Fourier Fishing.

2. **Decision problem separation (conditional):** Assuming the Generalized Linial-Nisan Conjecture (that almost $k$-wise independence fools AC$^0$), there exists an oracle $A$ with BQP$^A \not\subseteq$ PH$^A$. The underlying problem is Fourier Checking.

3. **Unconditional oracle separations:** BQP$^A \not\subseteq$ BPP$^A_{\text{path}}$ and BQP$^A \not\subseteq$ SZK$^A$ for some oracle $A$, via Fourier Checking without any conjecture.

My assessment: This paper is a landmark. It introduced Forrelation as a concept, drew the connection between quantum-classical separations and circuit lower bounds, and set up the technical programme that Aaronson-Ambainis (2015) and ultimately Raz-Tal (2019) completed. The Generalized Linial-Nisan Conjecture was proved by Tal (2017), and Raz-Tal (2019) used it to give the unconditional oracle separation BQP $\not\subseteq$ PH — vindicating the programme laid out here.

Fourier Checking is "the same problem as Forrelation" — the later Aaronson-Ambainis 2015 paper refined and optimised the query complexity analysis. This 2010 paper is the conceptual origin; the 2015 paper is the optimal query separation.

---

## The quantum algorithms

### Fourier Fishing (FF-ALG)

For each $i = 1, \ldots, n$:
1. Prepare $\frac{1}{\sqrt{N}}\sum_{x} f_i(x)|x\rangle$ (one phase query to $f_i$).
2. Apply $H^{\otimes n}$.
3. Measure → output $z_i$.

This samples $z_i$ with probability $\hat{f}_i(z_i)^2 / N$, biased toward large Fourier coefficients. By concentration inequalities, this satisfies the Fourier Fishing requirements with probability $1 - 1/\exp(n)$.

### Fourier Checking (FC-ALG)

1. Prepare $\frac{1}{\sqrt{N}}\sum_x |x\rangle$.
2. Query $f$: $\frac{1}{\sqrt{N}}\sum_x f(x)|x\rangle$.
3. Apply $H^{\otimes n}$: $\frac{1}{N}\sum_{x,y} f(x)(-1)^{x \cdot y}|y\rangle$.
4. Query $g$: $\frac{1}{N}\sum_{x,y} f(x)(-1)^{x \cdot y}g(y)|y\rangle$.
5. Apply $H^{\otimes n}$, measure. Accept iff outcome is $|0^n\rangle$.

The acceptance probability is:
$$p(f,g) = \frac{1}{N^3}\left|\sum_{x,y} f(x)(-1)^{x \cdot y}g(y)\right|^2$$

For uniform $\langle f, g \rangle$: $\mathbb{E}[p] = 1/N$ (exponentially small). For forrelated $\langle f, g \rangle$: $\mathbb{E}[p] > 0.07$ (constant). So $O(1)$ queries suffice.

This is exactly the [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes|Forrelation]] circuit — the same $H$-$O_f$-$H$-$O_g$-$H$-measure structure.

---

## Classical hardness

### Fourier Fishing ∉ FBPP$^{\text{PH}}$ (unconditional)

The argument reduces $\varepsilon$-Bias Detection (distinguishing a fair coin from a biased coin) to Fourier Fishing via AC$^0$ circuits. The reduction works by:

1. **Secretly biasing a Fourier coefficient.** One can slightly bias a random Boolean function $f$ so that a specific Fourier coefficient $\hat{f}(s)$ is shifted by $O(1/\sqrt{N})$, while the truth table of $f$ remains information-theoretically indistinguishable from uniform. An adversary can't tell which coefficient was biased.

2. **Smuggling Majority into Fourier sampling.** If a FBPP$^{\text{PH}}$ machine could solve Fourier Fishing, it would output $z$ values correlated with large Fourier coefficients. The secret bias means the machine preferentially outputs $z = s$ — but computing $\hat{f}(s)$ is equivalent to computing Majority of $N$ bits. By Håstad's AC$^0$ lower bounds for Majority, this is impossible.

The core insight: computing any single Fourier coefficient requires summing $N$ bits (equivalent to Majority), and the AC$^0$-PH correspondence turns PH machines into AC$^0$ circuits.

### Fourier Checking: almost $k$-wise independence

The paper proves that for all $k \leq 2^{n/4}$, the forrelated distribution is $O(k^2/2^{n/2})$-almost $k$-wise independent. This means: conditioned on any $k$ values of $f$ and $g$, the posterior probability that $\langle f, g \rangle$ is forrelated is still $1/2 \pm O(k^2/2^{n/2})$.

Since PH machines correspond to AC$^0$ circuits, and AC$^0$ circuits can be fooled by $k$-wise independence (the Linial-Nisan Conjecture, proved by Braverman 2010), the paper conjectures that almost $k$-wise independence also fools AC$^0$ — the **Generalized Linial-Nisan Conjecture**.

---

## Key results

| Result | Statement |
|---|---|
| Relation separation | $\exists A$: FBQP$^A \not\subseteq$ FBPP$^{\text{PH}^A}$ (unconditional) |
| Decision separation | $\exists A$: BQP$^A \not\subseteq$ PH$^A$ (conditional on GLN Conjecture) |
| BPP$_\text{path}$ separation | $\exists A$: BQP$^A \not\subseteq$ BPP$^A_{\text{path}}$ (unconditional) |
| SZK separation | $\exists A$: BQP$^A \not\subseteq$ SZK$^A$ (unconditional) |
| Almost $k$-wise independence | Forrelated distribution is $O(k^2/2^{n/2})$-almost $k$-wise independent for $k \leq 2^{n/4}$ |
| Non-oracle consequence | $\exists$ relation problem in BQLOGTIME $\setminus$ AC$^0$ |

---

## Comparison with prior work

| Paper | Separation | Technique |
|---|---|---|
| [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes\|Bernstein-Vazirani (1993)]] | BPP $\neq$ BQP oracle | Recursive Fourier Sampling |
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes\|Simon (1994)]] | Exponential BQP vs BPP | Hidden period structure |
| [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes\|BBBV (1997)]] | NP $\not\subseteq$ BQP oracle | Hybrid argument |
| [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes\|Watrous (2000)]] | BQP $\not\subseteq$ MA oracle | Group non-membership |
| **This paper (2010)** | **FBQP $\not\subseteq$ FBPP$^{\text{PH}}$ oracle; BQP $\not\subseteq$ PH conditional** | **Fourier Fishing/Checking** |
| [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes\|Aaronson-Ambainis (2015)]] | Optimal 1-vs-$\tilde{\Omega}(\sqrt{N})$ query gap | Refined Forrelation analysis |
| Raz-Tal (2019) | BQP $\not\subseteq$ PH oracle (unconditional) | Proved GLN Conjecture + this framework |

---

## The programme that this paper started

The paper explicitly lays out a research programme:

1. ✅ Prove the Generalized Linial-Nisan Conjecture → **Done** by Tal (2017)
2. ✅ Get unconditional BQP $\not\subseteq$ PH oracle → **Done** by Raz-Tal (2019)
3. ✅ Optimal query separation for Forrelation → **Done** by Aaronson-Ambainis (2015)
4. ✅ Forrelation is maximally separating → **Done** by Raz-Tal (2019), who showed Forrelation achieves the largest possible quantum-classical query separation for any partial Boolean function

This makes the paper one of those rare cases where the research programme was laid out clearly and subsequently completed in full.

---

## Limits / caveats

- All separations are relative to an oracle. No unconditional separation BQP $\neq$ PH is known (and proving one would separate P from PSPACE, among other things).
- The relation problem separation doesn't amplify: FBQP success probability is $1 - 1/\exp(n)$ while any FBPP$^{\text{PH}}$ succeeds with probability at most $\sim 0.99$. It's open whether the constant can be pushed to an exponential gap.
- Fourier Checking as stated is a *distributional* or *promise* problem. Finding a natural *language* (not promise problem) in BQP $\setminus$ PH remains open.
- The paper asks: is there an explicit computational problem that "instantiates" Fourier Checking the way Factoring instantiates period finding? This is still open.

---

## Reusable ideas

1. **[[Forrelation as Quantum-Classical Separator]].** The forrelation value $\Phi_{f,g} = N^{-3/2}\sum_{x,y}f(x)(-1)^{x\cdot y}g(y)$ captures "how correlated is $g$ with the Fourier transform of $f$?" This single quantity defines the cleanest known quantum-classical separation: quantum algorithms compute it in $O(1)$ queries, classical algorithms need $\tilde{\Omega}(\sqrt{N})$.

2. **[[Secret Bias Smuggling for Lower Bounds]].** To prove a lower bound against classical simulation of a quantum sampling process, embed a hard computational problem (Majority) inside the sampling distribution by secretly biasing one output. The classical simulator must implicitly solve the hard problem to match the bias, but can't identify which output was biased.

3. **[[PH-to-AC0 Correspondence for Oracle Separations]].** If a PH machine solves a problem on a $2^n$-bit oracle, reinterpreting $\exists$ quantifiers as OR gates and $\forall$ as AND gives an AC$^0$ circuit of size $2^{\text{poly}(n)}$. So AC$^0$ lower bounds on the oracle string directly yield PH oracle separations — and the rich toolkit of circuit complexity (Håstad, Razborov-Smolensky, Braverman) becomes available.

---

## References within this paper

- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani (1993)]] — BQP definition; Recursive Fourier Sampling; first BPP $\neq$ BQP oracle
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|BBBV (1997)]] — NP $\not\subseteq$ BQP oracle
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — factoring in BQP; most known quantum algorithms are in NP $\cap$ coNP
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — Simon's problem, exponential quantum speedup
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL (2006)]] — Jones polynomial approximation is BQP-complete (mentioned as hard to interpret as evidence for BQP $\not\subseteq$ PH)
- Braverman (2010) — proved the original Linial-Nisan Conjecture (polylogarithmic independence fools AC$^0$)
- Håstad (1986) — AC$^0$ lower bounds for Parity/Majority; depth-$d$ circuits need $\exp(\Omega(n^{1/(d-1)}))$ size
- Linial-Nisan (1990) — conjectured that polylogarithmic independence fools AC$^0$

---

## Cross-links

### Paper notes
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]] — the refined, optimal version of this paper's Forrelation analysis
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]] — Recursive Fourier Sampling, BQP definition
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — Grover optimality, NP oracle separation
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — earlier exponential oracle separation (but inside NP)
- [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes]] — BQP $\not\subseteq$ MA oracle, an intermediate step in the hierarchy of separations
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — related quantum-classical separation via sampling hardness
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]] — BosonSampling, another approach to quantum advantage via computational complexity
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — earlier Aaronson work on quantum vs classical distinguishing

### Trick cards
- [[Forrelation as Quantum-Classical Separator]]
- [[Secret Bias Smuggling for Lower Bounds]]
- [[PH-to-AC0 Correspondence for Oracle Separations]]
- [[Gaussian Lifting for Query Lower Bounds]] — technique from the refined Aaronson-Ambainis analysis
- [[Near-Orthogonality of Standard-Fourier Basis Pairs]] — related structure underlying Forrelation's hardness
- [[Block-Multilinear Polynomial Simulation of Quantum Queries]] — general simulation theorem from Aaronson-Ambainis
