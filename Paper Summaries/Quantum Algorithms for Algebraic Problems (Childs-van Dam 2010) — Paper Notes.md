# Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes

> **Source:** Andrew M. Childs and Wim van Dam, "Quantum algorithms for algebraic problems," *Reviews of Modern Physics* **82**, 1–52 (2010); arXiv:0812.0380
> **Links:** [arXiv](https://arxiv.org/abs/0812.0380) · [journal](https://doi.org/10.1103/RevModPhys.82.1)
> **Tags:** #review #hidden-subgroup-problem #algebraic-algorithms #order-finding #abelian-HSP #nonabelian-HSP #query-complexity #number-theory #graph-isomorphism

---

## The computational problem

This is a survey of quantum algorithms that exploit **algebraic structure** to achieve speedups over classical computation. The central theme is the [[Hidden Subgroup Problem|hidden subgroup problem (HSP)]]: given a group $G$ and a function $f: G \to S$ that is constant on left cosets of some hidden subgroup $H \leq G$ and distinct across cosets, find generators for $H$.

The review covers:
- The **abelian HSP** and its well-known polynomial-time solution via the quantum Fourier transform, which subsumes Simon's problem, Shor's factoring, discrete logarithm, and Pell's equation
- The **non-abelian HSP**, which remains largely open and is the key obstacle to quantum graph isomorphism algorithms
- **Quantum query complexity** for algebraic problems, including order-finding and computing hidden shifts
- Connections to **lattice problems**, number theory (class group and unit group computations), and the hardness of certain non-abelian HSP instances via reductions from lattice problems

The survey is not just a list of results — it provides a unified framework and highlights the structural reasons why abelian HSP is tractable, why non-abelian HSP is hard, and which algebraic properties separate the tractable from the intractable.

---

## What the paper does

Childs and van Dam wrote this as an invited review for *Reviews of Modern Physics*, targeting an audience familiar with quantum computing but not necessarily experts in algebra. It organises the state of the field as of 2008–2009 around the HSP framework and identifies the key open problems.

The paper's structural contribution is showing that many algebraic exponential speedups covered by the survey — factoring, discrete logarithm, Simon's problem, Pell's equation, and later class-group/unit-group algorithms — fit an abelian-HSP or closely related period-finding template, while the non-abelian case (graph isomorphism being the flagship example) resists this approach for representation-theoretic and measurement-implementation reasons.

---

## Main results covered

### 1. The standard quantum algorithm for the abelian HSP

The core result underpinning the whole survey: for any finite abelian group $G$, the HSP can be solved with $O(\log |G|)$ queries and $\text{poly}(\log |G|)$ time.

**Algorithm sketch:**
1. Prepare $\frac{1}{\sqrt{|G|}} \sum_{g \in G} |g\rangle |0\rangle$
2. Evaluate $f$: $\frac{1}{\sqrt{|G|}} \sum_g |g\rangle |f(g)\rangle$
3. Measure the second register → first register collapses to a uniform superposition over a coset of $H$
4. Apply the QFT over $G$
5. Measure → obtain a uniformly random element of the dual subgroup $H^\perp$
6. Repeat $O(\log |G|)$ times; solve for $H$ using linear algebra

For abelian $G$ of the form $\mathbb{Z}_{N_1} \times \cdots \times \mathbb{Z}_{N_k}$, the QFT decomposes into independent QFTs over each factor and runs in $\text{poly}(\log |G|)$ time. See [[Coset Sampling via Fourier Transform]].

### 2. Instances of the abelian HSP

| Problem | Group $G$ | Hidden subgroup $H$ |
|---|---|---|
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] / Simon's problem | $\mathbb{Z}_2^n$ | Order-2 subgroup $\{0, s\}$ |
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] / factoring | $\mathbb{Z}^*_N$ (via period finding in $\mathbb{Z}$) | $\langle r \rangle$ where $r$ is the order of $x \bmod N$ |
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] / discrete log | $\mathbb{Z}_q \times \mathbb{Z}_q$ | Cyclic subgroup encoding the discrete log |
| [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] / abelian stabiliser | Any finite abelian group | Arbitrary $H \leq G$ |
| [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]] | $\mathbb{Z}_2^n$ | Hyperplane $\{x : s \cdot x = 0\}$ |
| [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]] / Pell's equation | discretized real-period function over $\mathbb{R}$ | regulator period $R\mathbb{Z}$, sampled approximately |

The Hallgren case is notable: the period is an irrational real number and the algorithm uses a discretized approximation to a weakly periodic real-domain function, not a literal finite abelian HSP over a listed group. The QFT is replaced by sampling from a continuous approximation, and the result is still polynomial-time.

### 3. Class group and unit group computations

Hallgren's 2002/2007 paper solves the regulator/Pell and PIP problems for real quadratic fields. Broader **class group** and **unit group** algorithms belong to later work such as Hallgren (2005) and Schmidt--Vollmer (2005), with additional assumptions and number-field machinery.

For class groups of imaginary quadratic fields, the hidden period lives in the group law of ideals. The QFT over $\mathbb{R}^k$ (for rank-$k$ unit groups) requires careful discretization and error analysis.

### 4. The non-abelian HSP: coset states and their limits

For non-abelian $G$, the natural approach generates **coset states** $\rho_H = \frac{1}{|H|} \sum_{h \in H} |g_0 h\rangle \langle g_0 h|$ (or their superpositions). These states carry complete information about $H$ — the Ettinger-Høyer-Knill theorem ([[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|2004]]) shows that $O(\text{poly}(\log |G|))$ queries suffice for any finite group. The bottleneck is **processing** the coset states, not acquiring them.

The structural obstacle: applying the QFT over a non-abelian group yields matrix-valued representations, not scalars. Extracting $H$ from the non-abelian Fourier coefficients requires identifying which irreducible representations of $G$ are supported, which can require exponential classical computation.

**Key non-abelian results covered:**

- **Solvable black-box group structural tasks** ([[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous 2001]]): polynomial-time quantum algorithms for order, membership, subgroup equality, normality testing, and related factor-group computations. This is not a general algorithm for arbitrary HSP over every solvable group.
- **Dihedral group** $D_N$: subexponential time $2^{O(\sqrt{\log N})}$ via a sieving algorithm ([[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg 2005]]), improved to polynomial space by [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]]. The dihedral HSP is related to worst-case lattice problems, suggesting hardness.
- **Symmetric group** $S_n$ (graph isomorphism): the standard approach fails — single-copy measurements on coset states are provably insufficient (Moore-Russell-Schulman 2005). Entangled multi-copy measurements ([[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam 2005]]) yield the [[Entangled Multi-Copy Measurements for Nonabelian HSP|pretty good measurement]] approach, efficient for some semidirect product groups but reducing to average-case subset sum for dihedral groups.
- **Semidirect products** $\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$: polynomial time when $r$ is constant, via the PGM on multiple coset state copies.

### 5. Quantum query complexity for order-finding and related problems

The review covers the query complexity (number of oracle calls, ignoring computation time) of:

- **Order-finding**: computing $\text{ord}(x)$ in a group given an oracle for group multiplication. Polynomial quantum query complexity — exponentially better than classical for black-box groups.
- **Hidden shift problem**: given $f, g: G \to S$ with $g(x) = f(x + s)$ for a hidden $s$, find $s$. For $G = \mathbb{Z}_N^k$, solvable efficiently. For non-abelian groups, harder.
- **Shifted Legendre symbol** and related problems over $\mathbb{F}_p$: quantum algorithms achieve polynomial query complexity where classical exponential is needed.

The key tool is the **non-abelian Fourier analysis** on the function values, combined with phase estimation ([[Quantum Phase Estimation|QPE]]) to read off eigenvalue information.

### 6. Graph isomorphism and the HSP over $S_n$

Graph isomorphism (GI) reduces to the HSP over the symmetric group $S_n$: the hidden subgroup is the automorphism group of the graph (or a coset thereof). The review analyses why the standard coset sampling approach fails:

- There are exponentially many subgroups of $S_n$
- Single-copy measurements on coset states cannot distinguish all subgroups efficiently (information-theoretic lower bound on distinguishability)
- Even exponentially many single-copy measurements may not suffice — the "pretty good measurement" collapses to classical computation for $S_n$

No polynomial-time quantum algorithm for GI is known, and this remains one of the central open problems in the field.

---

## Key results

1. **Abelian HSP completeness**: Every abelian HSP instance can be solved with $O(\log |G|)$ quantum queries and $\text{poly}(\log |G|)$ quantum gates. This is tight: $\Omega(\log |G|)$ queries are necessary.

2. **Polynomial query complexity for all finite groups** (Ettinger-Høyer-Knill): $O(\log^4 |G|)$ queries suffice for any finite group, but the algorithm requires exponential classical post-processing.

3. **Solvable black-box group tasks** (Watrous): order, membership, subgroup equality, normality testing, and abelian factor-group computations are quantum polynomial-time for solvable black-box groups.

4. **Dihedral HSP in subexponential time** (Kuperberg): $2^{O(\sqrt{\log N})}$ time for $D_N$.

5. **Dihedral hardness via lattice connections** (Regev): an efficient quantum algorithm for the relevant dihedral coset problem would imply efficient quantum algorithms for certain promised unique-SVP lattice problems, suggesting the problem is hard.

6. **Continuous-period and infrastructure algorithms** (Hallgren, Schmidt--Vollmer): Pell/regulator and PIP for real quadratic fields, and later class-group/unit-group algorithms for number fields, are solvable in quantum polynomial time in their stated models.

| Category | What is solved efficiently | What is not implied |
|---|---|---|
| Abelian HSP | finite abelian HSP and standard period-finding applications | nonabelian HSP by representation labels alone |
| Normal-HSP / special nonabelian cases | normal subgroups and selected group families with extra structure | arbitrary hidden subgroups in arbitrary nonabelian groups |
| Solvable black-box group algorithms | order, membership, equality, normality, and abelian factor-group computations | a blanket HSP solver for arbitrary solvable-group instances |
| General finite-group HSP query complexity | polynomially many queries/coset states | efficient measurement or post-processing |

7. **Graph isomorphism barrier**: No polynomial-time quantum algorithm for the HSP over $S_n$ is known, and structural results suggest the standard approach cannot work.

---

## Comparison with prior work

Before this review, the HSP literature was scattered across individual algorithm papers. The main prior review was Lomonaco's 2004 overview (less technical) and the Mosca chapter in the Quantum Computation and Quantum Information textbook. This paper's contribution is:

- A **unified presentation** of abelian and non-abelian HSP under a common framework
- Clear separation between query complexity results (where quantum wins across the board) and computational complexity results (where non-abelian groups often remain hard)
- Thorough coverage of **number-theoretic applications** including class groups and unit groups that were not in earlier surveys
- Analysis of the **dihedral HSP / lattice connection**, which was relatively new at writing time

The paper appeared contemporaneously with major advances in the polynomial hierarchy separation results, the quantum walk algorithms for element distinctness ([[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis 2007]]), and the beginnings of the QSVT framework — none of these are covered since they fall outside the algebraic problem scope.

---

## Limits / caveats

- **Scope is algebraic problems only**: the review explicitly excludes quantum walk-based algorithms (apart from Childs' own glued-trees construction), amplitude estimation, and simulation. The algebraic focus is a strength for depth but means many quantum speedups are out of scope.
- **Graph isomorphism is untouched**: despite GI being the primary non-abelian HSP motivation, no progress toward an efficient algorithm is reported. The paper is honest about this being a barrier.
- **Continuous groups require more machinery**: the Pell's equation and class group algorithms involve technical analysis of continuous QFTs over $\mathbb{R}^k$ that the review summarises but does not fully treat.
- **Post-2009 developments not covered**: the framework has been extended by QSVT (2018–2019), which subsumes some of these speedups in a unified framework. The non-abelian HSP status is essentially unchanged as of 2024.
- **Quantum walk-based search** is only lightly touched: the survey mostly stays on the algebraic side and doesn't develop the Szegedy / Ambainis frameworks.
- **Survey compression can overstate boundaries**: when using this note, keep separate abelian HSP, normal-HSP special cases, solvable black-box structural algorithms, and arbitrary nonabelian HSP.

---

## Reusable ideas

1. **[[HSP as Algorithmic Template (Abelian)]]** — The four-step recipe (superpose, evaluate, QFT, measure, repeat) is the reusable template behind almost all known exponential quantum speedups in algebra and number theory. Before applying a quantum algorithm to a new algebraic problem, check whether it fits the abelian HSP mould.

2. **[[Abelian vs Non-Abelian HSP Hardness Dichotomy]]** — The query vs. computational complexity separation for non-abelian groups is a design heuristic: the information is always accessible in polynomially many queries, but the computational bottleneck is in the measurement / post-processing. For non-abelian groups, always ask: does the group have a solvable structure (tractable), a dihedral or lattice connection (subexponential at best), or symmetric-group-like structure (likely hard)?

3. **[[Coset Sampling via Fourier Transform]]** — The fundamental subroutine: prepare a coset state, apply the QFT, measure to get a random element of the dual subgroup. The abelian case gives a scalar phase; the non-abelian case gives a matrix-valued representation. Reuse whenever you need to extract periodicity or symmetry from a black-box function on a group.

4. **[[Order-Finding via QFT and Continued Fractions]]** — The specific instantiation for cyclic groups: the QFT measurement gives a rational approximation $c/q \approx d/r$, and continued fractions recover $r$ from $O(\log\log N)$ measurements. Directly applicable to factoring, discrete log, and any problem that reduces to order-finding.

5. **Lattice-HSP reductions as hardness evidence** — The Regev reduction from lattice SVP to dihedral HSP provides a template for arguing hardness: if you can show a candidate problem is equivalent to a known hard HSP variant, that's strong evidence against efficient algorithms.

---

## References within this paper

Key papers cited in the review (those with notes in this vault):

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — $\mathbb{Z}_2^n$ HSP, the original quantum separation via algebraic structure
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — factoring and discrete log as abelian HSP
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — general abelian HSP via phase estimation
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani (1993)]] — hidden linear function, abelian HSP precursor
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes|Hallgren (2002)]] — continuous group HSP, Pell's equation
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]] — solvable black-box group structural algorithms
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger-Høyer-Knill (2004)]] — polynomial query complexity for all finite groups
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]] — dihedral HSP in subexponential time
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]] — dihedral HSP with polynomial space
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam (2005)]] — PGM for semidirect products (van Dam is a coauthor of this review)
- [[Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008) — Paper Notes|van Dam-Shparlinski (2008)]] — adjacent semidirect-product HSP motivation: solving $a f^x+b f^y=c$ efficiently would help the $\mathbb Z/q\rtimes\mathbb Z/p$ case, but their algorithm remains superpolynomial in $\log q$
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]] — dihedral HSP ↔ lattice problem hardness
- [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes|Watrous (2000)]] — group membership in QMA

---

## Cross-links

### Concept hubs
- [[Hidden Subgroup Problem]] — the central concept; this review is the definitive HSP survey
- [[Quantum Phase Estimation]] — Kitaev's phase estimation is the abelian HSP for cyclic groups

### Trick cards
- [[Coset Sampling via Fourier Transform]] — core subroutine for all HSP algorithms
- [[Order-Finding via QFT and Continued Fractions]] — the factoring / discrete log instantiation
- [[Entangled Multi-Copy Measurements for Nonabelian HSP]] — beyond single-copy for non-abelian groups
- [[HSP as Algorithmic Template (Abelian)]] — the four-step reusable recipe (created alongside this note)
- [[Abelian vs Non-Abelian HSP Hardness Dichotomy]] — design heuristic for new problems (created alongside this note)

### Paper notes
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — Simon's problem as HSP over $\mathbb{Z}_2^n$
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — factoring and discrete log
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — abelian stabilizer / phase estimation view
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]] — query complexity lower bound doesn't help
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]] — dihedral case
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]] — dihedral case, space-efficient
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]] — optimal measurements for structured non-abelian groups
- [[Classical and Quantum Algorithms for Exponential Congruences (van Dam-Shparlinski 2008) — Paper Notes]] — finite-field exponential congruence algorithm motivated by semidirect-product HSP
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — polynomial-time order, membership, equality, normality, and factor-group tools for solvable black-box groups
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]] — continuous group HSP
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]] — lattice hardness connection
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] — Childs' glued-trees construction (same first author; walk-based exponential speedup)
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — quantum walk speedup outside the HSP framework, for contrast
