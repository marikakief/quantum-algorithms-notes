# HSP as Algorithmic Template (Abelian)

> **Source:** Childs and van Dam, arXiv:0812.0380 (survey); originating in [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]], [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]], [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]]
> **Tags:** #trick #hidden-subgroup-problem #abelian #template #fourier #period-finding

## What it does

Converts any problem that can be encoded as "find a hidden subgroup of a finite abelian group" into a polynomial-time quantum algorithm. The four-step recipe is the common engine behind factoring, discrete logarithm, Simon's problem, Pell's equation, class groups, and unit groups.

## The trick

Given a finite abelian group $G$ and a function $f: G \to S$ that is constant on cosets of an unknown subgroup $H \leq G$ and distinct across cosets:

1. **Superpose**: prepare $\frac{1}{\sqrt{|G|}} \sum_{g \in G} |g\rangle |0\rangle$
2. **Evaluate**: apply the oracle to get $\frac{1}{\sqrt{|G|}} \sum_g |g\rangle |f(g)\rangle$
3. **QFT + measure**: apply the quantum Fourier transform over $G$ to the first register and measure; get a uniformly random element of the dual subgroup $H^\perp = \{\chi \in \hat{G} : \chi(h) = 1 \; \forall h \in H\}$
4. **Reconstruct**: repeat $O(\log |G|)$ times; $H^\perp$ generates a lattice from which $H$ is recovered by classical linear algebra

**Why it works**: After measuring the second register, the first register is in a uniform superposition over a coset $g_0 H$. The QFT maps this coset state to a uniform superposition over characters in $H^\perp$. Measuring then samples from $H^\perp$ uniformly, and $O(\log |G|)$ independent samples generate $H^\perp$ with high probability.

**Encoding new problems**: to apply this template to a new problem, identify:
- What is the group $G$?
- What is the subgroup encoding the answer?
- Can you evaluate $f$ (the "periodicity oracle") efficiently on a quantum computer?

If all three are yes, you have a polynomial-time quantum algorithm.

## When to reach for it

- Factoring: $G = \mathbb{Z}$, hidden period = order of $x \bmod N$ (map to cyclic group via modular exponentiation)
- Discrete logarithm: $G = \mathbb{Z}_q \times \mathbb{Z}_q$, hidden subgroup encodes the log
- Simon's problem: $G = \mathbb{Z}_2^n$, hidden subgroup $\{0, s\}$
- Pell's equation / principal ideal problem: $G = \mathbb{R}$ (continuous), hidden period = regulator
- Class groups and unit groups of number fields
- Any problem where the answer is the "period" or "symmetry" of a function on an abelian group

## Complexity

$O(\log |G|)$ quantum queries to $f$, each with a QFT of cost $O(\log^2 |G|)$ gates (or $O(\log |G| \log\log |G|)$ with the approximate QFT). Total quantum gates: $\text{poly}(\log |G|)$.

Classical post-processing: solving a linear system over the character group, $O(\log^2 |G|)$ classical bit operations.

## Caveat

The template breaks for **non-abelian** groups. The QFT over a non-abelian group produces matrix-valued Fourier coefficients (one per irreducible representation), and extracting $H$ from these coefficients requires identifying which representations are supported — a task that is computationally hard in general. For non-abelian groups, see [[Abelian vs Non-Abelian HSP Hardness Dichotomy]].

Also: the template requires quantum access to $f$ (i.e., $f$ must be efficiently computable on a quantum computer in superposition). If $f$ is only classically accessible, the approach gives no speedup.

## Related notes

- [[Hidden Subgroup Problem]] — concept hub
- [[Coset Sampling via Fourier Transform]] — the core subroutine (steps 1–3 in detail)
- [[Order-Finding via QFT and Continued Fractions]] — factoring/discrete-log instantiation
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]] — the survey
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]]
