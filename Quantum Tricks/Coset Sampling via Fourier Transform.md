# Coset Sampling via Fourier Transform

> **Source:** Simon, FOCS 1994 / SIAM J. Comput. 1997; generalized in Kitaev (1995), Shor (1994)
> **Tags:** #trick #hidden-subgroup #fourier #period-finding #fundamental

## What it does

Extracts information about a hidden subgroup $H \leq G$ by preparing a uniform superposition over a coset of $H$, applying the quantum Fourier transform over $G$, and measuring. The measurement outcome is a uniformly random element of the dual subgroup $H^\perp$ (or its analogue in the character theory of $G$).

## The trick

Given a function $f: G \to S$ that is constant on cosets of $H$ and distinct on different cosets:

1. Prepare $\frac{1}{\sqrt{|G|}} \sum_{g \in G} |g\rangle|0\rangle$
2. Evaluate: $\frac{1}{\sqrt{|G|}} \sum_{g} |g\rangle|f(g)\rangle$
3. Measure the second register → first register collapses to uniform superposition over a coset $g_0 H$
4. Apply QFT over $G$ to the first register
5. Measure → get a random element of $H^\perp$
6. Repeat $O(\log|G|)$ times, solve for $H$ classically

**For $G = \mathbb{Z}_2^n$ (Simon's problem):** The QFT is just $H^{\otimes n}$. $H^\perp = s^\perp$ is an $(n-1)$-dimensional subspace. Solve linear system over $\mathbb{F}_2$.

**For $G = \mathbb{Z}_N$ (Shor's algorithm):** The QFT is the standard QFT modulo $N$. $H^\perp$ consists of multiples of $N/r$ where $r = |H|$ is the period. Extract $r$ via continued fractions.

## When to reach for it

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon's problem]]: hidden subgroup of $\mathbb{Z}_2^n$
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Factoring / discrete log]]: hidden subgroup of $\mathbb{Z}_N$ or $\mathbb{Z}_N \times \mathbb{Z}_N$
- Any Abelian [[Hidden Subgroup Problem]]: graph isomorphism reduction attempts, lattice problems
- Quantum algorithms that need to extract periodicity or symmetry from a black-box function

## Complexity

$O(\log|G|)$ queries to $f$ and QFT applications. The QFT over $\mathbb{Z}_2^n$ costs $O(n)$ gates; over $\mathbb{Z}_N$ it costs $O(n^2)$ gates (or $O(n \log n)$ with approximate QFT).

## Caveat

Only works efficiently for **Abelian** groups. For non-Abelian groups (e.g., the symmetric group $S_n$, relevant to graph isomorphism), coset sampling produces matrix-valued representations rather than scalar phases, and the classical post-processing becomes exponentially hard in general. The non-Abelian hidden subgroup problem remains one of the major open problems in quantum algorithms.

## Related notes

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Interference-Based Period Finding]]
- [[Gapped Phase Estimation]]
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — extends coset sampling to non-abelian solvable groups via subnormal chains
- [[Subgroup State Preparation via Subnormal Chain]]
