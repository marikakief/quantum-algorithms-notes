# Infrastructure of Real Quadratic Fields as Period Domain

> **Source:** Sean Hallgren, STOC 2002 / JACM 54(1), 2007; based on Shanks (1972) and Lenstra (1982)
> **Tags:** #trick #number-theory #period-finding #algebraic-structure

## What it does
Converts the problem of computing the regulator of a real quadratic number field into a period-finding problem by exploiting the "infrastructure" of reduced principal ideals.

## The trick
The algebraic integers $\mathcal{O}$ of $\mathbb{Q}[\sqrt{d}]$ have a finite set of *reduced principal ideals* arranged in a cycle (the **principal cycle**) of circumference $R$ (the regulator). Each ideal has a compact $O(\log d)$-bit description $(a, b)$ with $0 \leq a, b < \sqrt{D}$.

Three operations make this infrastructure navigable:

1. **Stepping:** The reduction operator $\rho$ moves from one reduced ideal to the next, with distance increment $\delta(\rho(I)) - \delta(I) \in [3/(32D), \frac{1}{2}\ln D]$. Computable in $\text{poly}(\log D)$ time.

2. **Jumping:** The product operation $I \ast J$ computes the first reduced ideal at distance $> \delta(I) + \delta(J)$, in $\text{poly}(\log D)$ time. Iterated squaring reaches distance $> 2^n \ln 2$ in $\text{poly}(\log D, n)$ steps.

3. **Function evaluation:** Define $h(x) = (I_x, \tilde{x} - \delta(I_x))$ where $I_x$ is the nearest ideal to the left of $x$ on the cycle. This function has period $R$, is one-to-one within each period, and is efficiently computable using the jumping operation for large distances followed by stepping for fine positioning.

The result: the regulator $R$ — an algebraic invariant with no obvious periodic structure — becomes the period of an explicit, efficiently computable function.

## When to reach for it
- Computing regulators, fundamental units, class groups of quadratic number fields
- Any problem reducible to period-finding over $\mathbb{R}$ where the periodic structure comes from algebraic number theory
- Breaking cryptosystems based on the hardness of computing regulators (e.g., Buchmann-Williams key exchange)

## Complexity
All operations (stepping, jumping, function evaluation) run in $\text{poly}(\log D)$ time. The principal cycle has $\Theta(R / \ln D)$ to $\Theta(R / \ln 2)$ elements — exponentially many — but we never need to enumerate them.

## Caveat
- Specific to real quadratic fields ($d > 0$). Imaginary quadratic fields ($d < 0$) have finite class groups but no infrastructure in the same sense.
- The distance between consecutive reduced ideals can be as small as $3/(32D)$, which is exponentially small. This means the discretisation for [[Period Finding for Irrational Periods on R]] must be correspondingly fine.
- Extension to higher-degree number fields (Hallgren, STOC 2005) requires more involved algebraic machinery.

## Related notes
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]]
- [[Period Finding for Irrational Periods on R]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
