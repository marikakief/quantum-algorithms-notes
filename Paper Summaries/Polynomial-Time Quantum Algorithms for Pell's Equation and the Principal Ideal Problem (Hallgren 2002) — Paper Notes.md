> **Source:** Sean Hallgren, *Polynomial-time quantum algorithms for Pell's equation and the principal ideal problem*, Proc. 34th ACM STOC, pp. 653–658 (2002); Journal version: J. ACM 54(1), Article 4 (2007)
> **Links:** [JACM](https://dl.acm.org/doi/10.1145/1206035.1206039) · [Jozsa's notes (arXiv)](https://arxiv.org/abs/quant-ph/0302134)
> **Tags:** #hidden-subgroup-problem #period-finding #number-theory #foundational

---

## The computational problem

**Pell's equation:** Given a square-free integer $d$, find the fundamental solution to $x^2 - dy^2 = 1$. Equivalently, compute the *regulator* $R = \ln(x_0 + y_0\sqrt{d})$ of the real quadratic number field $\mathbb{Q}[\sqrt{d}]$, where $(x_0, y_0)$ is the smallest positive solution.

The fundamental solution can be exponentially large in $d$ (e.g., $d = 61$ gives $x_0 = 1766319049$), so even writing it down can take $O(\sqrt{d})$ digits — exponential in the input size $\log d$. The regulator $R$ has $O(\log d)$ digits, making it the right computational target.

**Principal ideal problem:** Given an ideal $I$ of the algebraic integers $\mathcal{O}$ in $\mathbb{Q}[\sqrt{d}]$, determine whether $I$ is principal (i.e., of the form $\gamma\mathcal{O}$ for some $\gamma$) and if so, find a generator.

Both problems are equivalent in computational complexity. The best classical algorithms run in subexponential time $O(\exp(\sqrt{\log d}))$, assuming the Generalised Riemann Hypothesis. Without GRH, the best classical bound is $O(d^{1/4})$.

## What the paper does

Hallgren gives a polynomial-time quantum algorithm for computing the regulator $R$ to any desired precision. This is a major extension of quantum [[Interference-Based Period Finding|period finding]] beyond the integers — the period $R$ is *irrational* and lives on $\mathbb{R}$, not on a finite cyclic group. The algorithm runs in $\text{poly}(\log d, n)$ time to compute $R$ to $n$ digits, succeeding with probability $1/\text{poly}(\log d)$.

This was the first quantum algorithm to solve a computational problem in algebraic number theory, opening a new domain for quantum speedups beyond factoring and discrete logarithms.

## The algorithm

The algorithm has two essentially independent parts: an algebraic setup that converts the problem into period-finding, and a quantum algorithm that finds irrational periods on $\mathbb{R}$.

### Part 1: Algebraic setup (classical)

The algebraic integers $\mathcal{O}$ in $\mathbb{Q}[\sqrt{d}]$ form a ring with a finite (but exponentially large) set of *reduced principal ideals* $\{J_0 = \mathcal{O}, J_1, \ldots, J_{k_0-1}\}$ called the **principal cycle**. Each $J_i$ has a compact description as a pair of integers $(a_i, b_i)$ with $0 \leq a_i, b_i < \sqrt{D}$ (where $D$ is the discriminant), so each ideal has a $\text{poly}(\log d)$-sized representation.

Key properties of the principal cycle:
- **Reduction operator** $\rho$: maps each reduced ideal to the next one on the cycle, computable in $\text{poly}(\log D)$ time
- **Distance function** $\delta(J_i)$: the distance of $J_i$ from $\mathcal{O}$ along the cycle, with total circumference $R$
- **Product operation** $I \ast J$: computes the first reduced ideal with distance exceeding $\delta(I) + \delta(J)$, enabling exponentially large distance jumps via iterated squaring in $\text{poly}(\log D, n)$ time

Hallgren constructs a function $h: \mathbb{R} \to \text{PIred} \times \mathbb{R}$ that maps $x$ to the pair (nearest reduced ideal to the left of $x$, distance gap). This function is:
1. Periodic with period $R$ (the regulator)
2. One-to-one within each period
3. Efficiently computable: given $x = k/N$, computable in $\text{poly}(\log D, \log x, n)$ time

### Part 2: Quantum period finding for irrational periods on $\mathbb{R}$

The standard [[Quantum Fourier Transform Circuit|QFT]]-based period finding works for integer periods on cyclic groups. Hallgren needs to handle:
- The period $R$ is irrational
- The domain is $\mathbb{R}$ (not finitely generated)

The approach: discretise $h$ by evaluating at multiples of $1/N$ for large $N$. The discretised function $\tilde{h}_N$ is **weakly periodic** — it satisfies $\tilde{h}_N(k) = \tilde{h}_N(k + \lfloor lNR \rceil)$ for most $k$ and all $l$, with failures only at the $\leq 1/\log d$ fraction of $k$ values nearest to each reduced ideal.

The quantum algorithm:
1. Prepare the uniform superposition $\frac{1}{\sqrt{q}} \sum_{m=0}^{q-1} |m\rangle |f(m)\rangle$ with $q \geq 3(NR)^2$
2. Measure the second register, collapsing to a superposition over a weakly periodic set of positions
3. Apply the [[Quantum Fourier Transform Circuit|QFT]] mod $q$ and measure, obtaining $j$ near an integer multiple of $q/(NR)$
4. Run twice to get two such values $c, d$. Use [[Order-Finding via QFT and Continued Fractions|continued fraction expansion]] of $c/d$ to extract an approximation $m$ to $NR$
5. Verify using the efficient classical ideal computation: check if $m$ is within 1 of an integer multiple of $NR$ by locating the nearest ideal

This gives $|NR - m| < 1$, i.e., $R$ to accuracy $1/N$.

## Key results

**Theorem (Hallgren 2002).** There is a quantum algorithm that computes the regulator $R$ of $\mathbb{Q}[\sqrt{d}]$ to accuracy $10^{-n}$ in time $\text{poly}(\log d, n)$ with success probability $1/\text{poly}(\log d, n)$.

**Corollary.** Pell's equation, the principal ideal problem, and the class group of $\mathbb{Q}[\sqrt{d}]$ can all be solved in quantum polynomial time.

## Comparison with prior work

| Problem | Best classical | Hallgren's quantum |
|---|---|---|
| Pell's equation / regulator | $\exp(O(\sqrt{\log d}))$ (assuming GRH) | $\text{poly}(\log d)$ |
| Principal ideal problem | $\exp(O(\sqrt{\log d}))$ (assuming GRH) | $\text{poly}(\log d)$ |
| Class group computation | $\exp(O(\sqrt{\log d}))$ (assuming GRH) | $\text{poly}(\log d)$ |
| Factoring | $\exp(O((\log N)^{1/3}))$ (NFS) | $\text{poly}(\log N)$ ([[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes\|Shor]]) |

Pell's equation is *harder* than factoring classically ($\exp(\sqrt{\log d})$ vs $\exp((\log N)^{1/3})$), and factoring reduces to computing regulators. So Hallgren's result handles a strictly harder problem than Shor's.

## Limits / caveats

- The algorithm is for *real quadratic* number fields ($\mathbb{Q}[\sqrt{d}]$ with $d > 0$). It does not directly apply to imaginary quadratic fields or higher-degree number fields.
- Hallgren later extended to unit groups and class groups of arbitrary-degree number fields (STOC 2005), but those algorithms are more involved.
- The success probability is $1/\text{poly}(\log d)$ per run, requiring $O(\text{poly}(\log d))$ repetitions. Standard amplification applies.
- The weak periodicity notion tolerates a $1/\log d$ fraction of "bad" points. This works because the QFT output probabilities are insensitive to small perturbations of the input state.
- The connection to lattice problems (via Regev's subsequent work) gives the dihedral HSP and lattice-based cryptography as downstream implications. Solving the dihedral HSP efficiently would break certain lattice-based crypto.

## Reusable ideas

1. [[Period Finding for Irrational Periods on R]] — generalising QFT-based period finding from finite cyclic groups to $\mathbb{R}$ with irrational periods, via discretisation and weak periodicity
2. [[Infrastructure of Real Quadratic Fields as Period Domain]] — using the principal cycle of reduced ideals as a computational domain where the regulator appears as a period, with efficient navigation via iterated squaring of ideal products

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — the period-finding paradigm that Hallgren generalises
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation, which underpins the QFT approach
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — Simon's problem, an earlier HSP on $(Z/2)^n$
- Buchmann-Williams (1989) — key exchange based on the hardness of computing regulators (broken by Hallgren's algorithm)
- Lenstra (1982) — the classical theory of reduced ideals and the infrastructure of quadratic fields
- Shanks (1972) — introduced the distance function on the principal cycle of reduced ideals
- Jozsa (2003, arXiv:quant-ph/0302134) — detailed exposition of Hallgren's algorithm with self-contained algebraic number theory

## Cross-links

### Paper notes
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — Shor's algorithm; Hallgren generalises the period-finding component
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase estimation framework
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]] — unified phase estimation / QFT framework
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — Simon's problem as HSP precursor
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]] — HSP for semidirect products; dihedral case discussed
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — quantum algorithms for group-theoretic problems

### Trick cards
- [[Period Finding for Irrational Periods on R]]
- [[Infrastructure of Real Quadratic Fields as Period Domain]]
- [[Interference-Based Period Finding]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Quantum Fourier Transform Circuit]]
- [[Coset Sampling via Fourier Transform]]
