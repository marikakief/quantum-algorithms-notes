
> **Source:** Deutsch & Jozsa, Proc. R. Soc. Lond. A 439 (1992)
> **Tags:** #trick #oracle #interference #foundational

## What it does
Detects global properties of a Boolean function (e.g., constant vs. balanced) by sandwiching a phase oracle between two layers of Hadamard transforms, so that the function values interfere constructively or destructively on the all-zeros outcome.

## The trick

$$|0^n\rangle \xrightarrow{H^{\otimes n}} \frac{1}{\sqrt{2^n}} \sum_x |x\rangle \xrightarrow{(-1)^{f(x)}} \frac{1}{\sqrt{2^n}} \sum_x (-1)^{f(x)} |x\rangle \xrightarrow{H^{\otimes n}} |\psi\rangle$$

The amplitude on $|0^n\rangle$ in the final state is the normalised sum of phases:

$$\alpha_{0^n} = \frac{1}{2^n} \sum_x (-1)^{f(x)}$$

This is the **zeroth Fourier coefficient** of $(-1)^{f(x)}$. If all phases agree (constant $f$), $|\alpha_{0^n}| = 1$. If phases cancel (balanced $f$), $\alpha_{0^n} = 0$. Measuring distinguishes the two cases with certainty.

The same structure, with $H^{\otimes n}$ replaced by the QFT, detects periodicity — this is how [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon's algorithm]] and [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]] work. The Hadamard sandwich is the special case where the relevant group is $\mathbb{Z}_2^n$.

## When to reach for it
- Detecting any "global" property that corresponds to the Fourier spectrum of $f$ being concentrated or spread
- As the conceptual starting point for understanding QFT-based algorithms
- Teaching quantum algorithms — this is the simplest nontrivial example of quantum interference doing useful computation

## Complexity
Two layers of $H^{\otimes n}$ ($O(n)$ gates) plus one oracle query. Total: $O(n) + 1$ query.

## Caveat
Only works for properties that show up in the Fourier spectrum. For unstructured problems (no algebraic structure to the function), this doesn't help — that's where [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover's algorithm]] (amplitude amplification) takes over instead.

## Related notes
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]]
- [[Phase Kickback from Oracle Queries]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Coset Sampling via Fourier Transform]]
