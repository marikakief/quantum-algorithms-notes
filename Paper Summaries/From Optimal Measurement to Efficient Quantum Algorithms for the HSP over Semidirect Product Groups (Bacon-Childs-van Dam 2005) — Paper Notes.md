> **Source:** Dave Bacon, Andrew M. Childs, and Wim van Dam, *From optimal measurement to efficient quantum algorithms for the hidden subgroup problem over semidirect product groups*, Proc. 46th IEEE FOCS, pp. 469–478 (2005); arXiv:quant-ph/0504083
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0504083)
> **Tags:** #hidden-subgroup-problem #nonabelian #pretty-good-measurement #entangled-measurement #foundational

---

## The computational problem

**Hidden subgroup problem (HSP):** Given a function $f: G \to S$ that is constant and distinct on left cosets of an unknown subgroup $H \leq G$, find generators for $H$ using oracle queries to $f$.

This paper focuses on semidirect product groups $G = A \rtimes_\varphi \mathbb{Z}_p$ where $A$ is abelian and $p$ is prime. Includes:
- Metacyclic groups $\mathbb{Z}_N \rtimes \mathbb{Z}_p$
- Groups $\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$ for fixed $r$ (including the Heisenberg group at $r=2$)
- Dihedral groups as a special case ($A = \mathbb{Z}_N$, $p = 2$, $\varphi(a) = -a$)

---

## What the paper does

Uses the **pretty good measurement** (PGM, also known as the square root measurement) as the key tool for distinguishing hidden subgroup states. Shows:

1. For semidirect products $A \rtimes \mathbb{Z}_p$, the PGM is *optimal* for distinguishing hidden subgroup states.
2. The implementation of the PGM reduces to solving an *average-case algebraic problem* called the **matrix sum problem**.
3. When the matrix sum problem can be solved efficiently, this yields efficient HSP algorithms.
4. Efficient algorithms for two new families: metacyclic groups (via discrete log) and $\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$ for fixed $r$ (via Gröbner bases).

The last family is significant: it's the **first efficient quantum algorithm to use entangled measurements across multiple copies** of hidden subgroup states. Prior algorithms only used single-copy measurements.

---

## The approach

### Reduction to cyclic subgroups

**Lemma 1:** For $G = A \rtimes \mathbb{Z}_p$ with $p$ prime, it suffices to distinguish between $H$ trivial and $H = \langle(d, 1)\rangle$ for unknown $d \in A$. This generalises Ettinger-Høyer's reduction for the dihedral case.

### Hidden subgroup states

After the standard approach (create superposition, query oracle, discard function register), the state on $k$ copies is:

$$
\tilde{\rho}_d^{\otimes k} = \frac{1}{|G|^k} \sum_{x \in A^k} \sum_{w,v \in A} \chi_w(d)\overline{\chi_v(d)} \sqrt{\eta_w^x \eta_v^x} |x, S_w^x\rangle\langle x, S_v^x|
$$

where $\eta_w^x = |\{b \in \mathbb{Z}_p^k : \hat{\Phi}(b)(x) = w\}|$ counts solutions to the **matrix sum problem** $\hat{\Phi}(b)(x) = w$.

### The matrix sum problem

For $x \in A^k$ and $w \in A$, find $b \in \mathbb{Z}_p^k$ satisfying:

$$
\hat{\Phi}(b)(x) = \sum_{j=1}^k \hat{\Phi}(b_j)(x_j) = w
$$

where $\hat{\Phi}$ is the Fourier dual of the automorphism-derived function $\Phi$.

Special cases:
- **Dihedral group:** reduces to average-case subset sum (hard — Kuperberg's subexponential algorithm)
- **Abelian case ($\varphi = \text{id}$):** reduces to $bx = w$ (easy)
- **$\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$:** reduces to polynomial equations over $\mathbb{F}_p$ (solvable via Gröbner bases for fixed $r$)
- **Metacyclic $\mathbb{Z}_N \rtimes \mathbb{Z}_p$:** reduces to discrete logarithm (solvable via [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]])

### Pretty good measurement

The PGM for ensemble $\{\sigma_j\}$ with equal priors is $E_j = \Sigma^{-1/2} \sigma_j \Sigma^{-1/2}$ where $\Sigma = \sum_j \sigma_j$.

The paper shows:
- The PGM is optimal for $k$ copies of hidden subgroup states over $A \rtimes \mathbb{Z}_p$.
- Its success probability is $\frac{1}{|A|} \sum_{x \in A^k} \left(\sum_{w \in A} \eta_w^x / p^k\right)^2$.
- It can be implemented as: Fourier transform on $A$, then *solve the matrix sum problem* coherently, then measure.

---

## Key results

| Group family | Copies needed | Matrix sum problem | Algorithm |
|---|---|---|---|
| Abelian (baseline) | 1 | $bx = w$ | Standard QFT |
| Metacyclic $\mathbb{Z}_N \rtimes \mathbb{Z}_p$, $N/p = \text{poly}(\log N)$ | 1 | Scalar DLP | Shor + PGM |
| $\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$, fixed $r$ | $r$ | Polynomial system over $\mathbb{F}_p$ | Gröbner bases + PGM |
| Heisenberg group ($r = 2$) | 2 | Quadratic equations over $\mathbb{F}_p$ | Special case of above |
| Dihedral $\mathbb{Z}_N \rtimes \mathbb{Z}_2$ | $O(\log N)$ | Subset sum | PGM optimal, but hard to implement |

---

## Comparison with prior work

| Paper | Groups solved |
|---|---|
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes\|Shor (1994)]] | Abelian HSP (factoring, discrete log) |
| [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes\|Watrous (2001)]] | Normal subgroups with efficient QFT |
| Ettinger-Høyer-Knill (1999–2004) | Dihedral HSP: polynomial query complexity, but no efficient algorithm |
| Kuperberg (2005) | Dihedral HSP: subexponential time |
| **This paper (2005)** | Metacyclic groups, $\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$; first entangled multi-copy algorithm |

---

## Limits / caveats

- The dihedral HSP remains open — the PGM is optimal but the matrix sum problem (subset sum) appears hard. Kuperberg's subexponential algorithm uses a different approach.
- The symmetric group HSP (for graph isomorphism) is not addressed. It requires fundamentally different techniques — single-register measurements don't suffice.
- For $\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$, the number of copies needed is $r$ (fixed). If $r$ grows with the problem size, the Gröbner basis computation becomes expensive.
- The reduction to cyclic subgroups (Lemma 1) requires $p$ prime. Composite orders need separate treatment.

---

## Reusable ideas

1. **[[Pretty Good Measurement for State Discrimination]]:** The PGM $E_j = \Sigma^{-1/2}\sigma_j\Sigma^{-1/2}$ is a universal tool for quantum state discrimination. It's often optimal or near-optimal, and its implementation reduces to diagonalising the average state $\Sigma$. In the HSP context, this diagonalisation reduces to an algebraic problem (the matrix sum problem), connecting quantum measurement design to classical algebra.

2. **[[Entangled Multi-Copy Measurements for Nonabelian HSP]]:** For the nonabelian HSP, measuring copies of the hidden subgroup state one at a time is provably insufficient (e.g., for the symmetric group). This paper demonstrates that entangled measurements across $r$ copies can succeed where single-copy measurements fail, using the PGM on $\rho_d^{\otimes r}$ as the entangled measurement. The number of copies needed equals the "rank" parameter $r$ of the group structure.

---

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — abelian HSP, discrete log for metacyclic case
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]] — normal HSP with QFT
- Ettinger, Høyer & Knill (1999–2004) — dihedral HSP query complexity, information-theoretic analysis
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]] — subexponential dihedral HSP via quantum sieve
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]] — polynomial-space variant of Kuperberg's algorithm
- Moore, Rockmore, Russell & Schulman (2004) — entanglement necessary for symmetric group HSP

---

## Cross-links

### Paper notes
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — normal HSP algorithms
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — Simon's problem as abelian HSP
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — factoring as abelian HSP
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]] — quantum sieve approach to dihedral HSP
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]] — polynomial-space dihedral HSP
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]] — period finding over real quadratic fields

### Trick cards
- [[Pretty Good Measurement for State Discrimination]] — the PGM technique
- [[Entangled Multi-Copy Measurements for Nonabelian HSP]] — multi-copy measurements
