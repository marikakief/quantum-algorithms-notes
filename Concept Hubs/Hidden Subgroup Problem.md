---
aliases:
  - HSP
  - hidden subgroup problem
---

# Hidden Subgroup Problem

**Concept hub** — the algebraic structure underlying most known exponential quantum speedups.

---

## What it is

Given a group $G$, a subgroup $H \leq G$, and a function $f: G \to S$ that is constant on left cosets of $H$ and distinct across cosets, find a generating set for $H$.

For **Abelian** groups, the quantum algorithm is:

1. Prepare $\frac{1}{\sqrt{|G|}}\sum_{g \in G} |g\rangle|0\rangle$
2. Evaluate $f$: $\frac{1}{\sqrt{|G|}}\sum_{g \in G} |g\rangle|f(g)\rangle$
3. Apply the quantum Fourier transform over $G$ to the first register
4. Measure — get a uniformly random element of $H^\perp$ (the dual/annihilator of $H$)
5. Repeat $O(\log|G|)$ times to generate $H^\perp$, then recover $H$

This is [[Coset Sampling via Fourier Transform]] — the common engine behind Simon's, Shor's, and Kitaev's algorithms.

---

## Abelian HSP: solved

Every finite Abelian group has an efficient QFT, so the Abelian HSP is solvable in polynomial time.

### Key instances

| Problem | Group | Hidden subgroup | Paper |
|---------|-------|----------------|-------|
| Simon's problem | $\mathbb{Z}_2^n$ | $\{0, s\}$ | [[On the Power of Quantum Computation (Simon 1994) — Paper Notes\|Simon (1994)]] |
| Factoring / discrete log | $\mathbb{Z}_N$ or $\mathbb{Z}_N \times \mathbb{Z}_N$ | $\langle r \rangle$ (order) | [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes\|Shor (1994)]] |
| Abelian stabiliser | any finite Abelian $G$ | arbitrary $H$ | [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes\|Kitaev (1995)]] |
| Bernstein-Vazirani | $\mathbb{Z}_2^n$ | hyperplane $s \cdot x = 0$ | [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes\|Bernstein-Vazirani (1993)]] |

---

## Non-Abelian HSP: mostly open

The coset sampling approach yields coset states $\rho_H = \frac{1}{|H|}\sum_{h \in H}|gH\rangle\langle gH|$, but for non-Abelian groups the QFT alone doesn't extract enough information. The main results:

- **Solvable groups** — polynomial-time via series of normal subgroups: [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]]
- **Symmetric group $S_n$** (graph isomorphism) — exponentially many coset samples needed for standard approach; the information is there but hard to extract
- **Dihedral group** — subexponential classical post-processing of quantum samples; connected to lattice problems

The non-Abelian HSP for $S_n$ is the main algebraic barrier to a quantum algorithm for graph isomorphism.

---

## Trick cards

- [[Coset Sampling via Fourier Transform]] — the core subroutine
- [[Order-Finding via QFT and Continued Fractions]] — the specific instantiation for cyclic groups (Shor's algorithm)

---

## Relationship to other concepts

- **[[Quantum Phase Estimation]]** — Kitaev's phase estimation is the Abelian HSP for cyclic groups, reframed as eigenvalue extraction from a unitary
- The HSP framework explains *why* factoring and discrete log have quantum speedups but doesn't (yet) extend to NP-hard problems
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett et al. (1997)]] — the oracle separation showing unstructured search has no exponential speedup, contrasting with the HSP speedups
