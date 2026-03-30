# Collision-Based Classical Lower Bound for Field-Structured Problems

> **Source:** de Beaudrap, Cleve, Watrous, arXiv:quant-ph/0011065
> **Tags:** #trick #lower-bound #query-complexity #finite-fields #birthday-paradox

## What it does
Proves $\Omega(\sqrt{q})$ classical query lower bounds for problems over finite fields $\mathrm{GF}(q)$ where the only way to extract information is through output collisions.

## The trick
Consider a black-box problem over $\mathrm{GF}(q)$ where the oracle output is $\pi(y_i + sx_i)$ for an unknown permutation $\pi$ and unknown $s \in \mathrm{GF}(q)$. After $k$ distinct queries:

**With a collision** (two outputs agree): Since $\pi$ is injective, $y_i + sx_i = y_j + sx_j$, so $s = (y_i - y_j)/(x_j - x_i)$. Division works because we're in a *field*. One collision determines $s$ exactly.

**Without a collision:** By symmetry, all $q - \binom{k}{2}$ remaining values of $s$ are equally likely. The output values are consistent with any of them.

The probability of a collision at query $k$, given none so far, is at most:

$$\frac{k-1}{q - k^2/2} \leq \frac{2k}{2q - k^2}$$

Summing: the probability of *any* collision in the first $l$ queries is at most $l^2/(2q - l^2)$. For this to reach $1/2$, we need $l \geq \sqrt{2q/3} = \Omega(\sqrt{q})$.

The structure of the argument is a birthday-type bound, but adapted to the algebraic setting where collisions are the *only* information channel.

## When to reach for it
- Proving classical lower bounds for oracle problems where the oracle output is scrambled by an unknown permutation
- Problems over finite fields where the hidden parameter enters linearly and the field structure prevents partial information extraction (no zero divisors)
- Constructing quantum-classical query separations: pair this lower bound with a QFT-based quantum algorithm

## Complexity
Gives $\Omega(\sqrt{q})$ classical queries, which for $q = 2^n$ is $\Omega(2^{n/2})$. Tight up to constants — the birthday bound is essentially optimal for collision-based information extraction.

## Caveat
The argument breaks completely for rings with zero divisors. For $\mathbb{Z}_{2^n}$, a binary search determines $s$ bit-by-bit in $n+1$ queries: query $(2^{n-1}, 0)$ to determine the parity of $s$, then refine. The division step $s = (y_i - y_j)/(x_j - x_i)$ requires invertibility, which needs a field.

The lower bound also applies only to deterministic algorithms (extended to randomised by Yao's minimax lemma). It says nothing about quantum query complexity — the quantum algorithm bypasses collisions entirely via the [[QFT Over Finite Fields with Control-Target Inversion]].

## Related notes
- [[Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) — Paper Notes]]
- [[QFT Over Finite Fields with Control-Target Inversion]]
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — the $\Omega(\sqrt{N})$ search lower bound, another collision-style argument
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — similar collision-based lower bound for Simon's problem
