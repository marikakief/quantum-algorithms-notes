# Bernoulli Polynomial Kernel Expansion for Product Formulas

> **Source:** Bagherimehrab, Berry et al., arXiv:2409.08265; originally from Laskar & Robutel (2001)  
> **Tags:** #trick #product-formulas #BCH #commutators #perturbation

## What it does

Gives an exact closed-form expression for the leading error terms of PF2 (and generalisations) in terms of Bernoulli polynomials, isolating exactly which nested commutator terms dominate the error at each order.

## The trick

For the second-order product formula with splitting parameter $s$:

$$e^{s\lambda A} e^{\lambda B} e^{(1-s)\lambda A} = \exp\!\left(\lambda(A + B) + \sum_{j=1}^{\infty} \frac{B_j(s)}{j!} \lambda^j \mathrm{ad}_A^j(B) + O(B^2)\right),$$

where $B_j(s)$ are Bernoulli polynomials and the $O(B^2)$ terms involve at least two powers of $B$ in the nested commutators.

For the symmetric PF2 ($s = 1/2$), all odd Bernoulli polynomial terms vanish: $B_j(1/2) = 0$ for odd $j$. The surviving error terms are:

| Order $2j$ | Bernoulli value $B_{2j}(1/2)$ | Error term |
|---|---|---|
| 2 | $-1/12$ | $-\frac{1}{24} \lambda^2 \mathrm{ad}_A^2(B)$ |
| 4 | $7/240$ | $\frac{7}{5760} \lambda^4 \mathrm{ad}_A^4(B)$ |
| 6 | $-31/1344$ | $-\frac{31}{967680} \lambda^6 \mathrm{ad}_A^6(B)$ |

This tells you exactly what to cancel: the error at order $\lambda^{2j+1}$ is dominated by $\mathrm{ad}_A^{2j}(B)$ with a known coefficient. The [[Symplectic Corrector Injection for Product Formulas|symplectic corrector]] targets these terms directly.

## When to reach for it

- Analysing the error structure of any product formula with two partitions
- Designing correctors for perturbed systems where first-order-in-$B$ terms dominate the error
- Understanding why PF2 has time-reversal symmetry (odd terms vanish) at the BCH level
- Extending results to Yoshida-type formulas where the kernel analysis at each order follows the same structure

## Complexity

This is an analytical identity — no computational cost. It converts the analysis problem from "expand the full BCH series and hope for cancellations" to "read off Bernoulli polynomial values from a table."

## Caveat

The expansion is modulo terms of degree $\ge 2$ in $B$. For non-perturbed systems ($\alpha = 1$), those second-order terms matter — they contain $\mathrm{ad}_B^2(A)$ and cross-terms that the symplectic corrector doesn't touch. For structured systems where $[B, [A, B]] = 0$ (e.g., $H = T + V(x)$ with quadratic kinetic energy), the expansion is exact to all orders in $B$ at each $\lambda$ order.

## Related notes
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]]
- [[Symplectic Corrector Injection for Product Formulas]]
- [[Vandermonde Compilation of Nested Commutator Exponentials]]
- [[Finite Nested-Commutator Expansion]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
