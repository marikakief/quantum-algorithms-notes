# Vandermonde Compilation of Nested Commutator Exponentials

> **Source:** Bagherimehrab, Berry et al., arXiv:2409.08265; building on Childs & Wiebe (2013) and Chen, Childs et al. (2022)  
> **Tags:** #trick #product-formulas #commutators #compilation #quantum-control

## What it does

Decomposes $\exp(C)$ where $C$ is a linear combination of nested commutators $\mathrm{ad}_A^{2j-1}(B)$ into a product of elementary exponentials $e^{a\lambda A}$ and $e^{b\lambda B}$, with all compilation parameters determined by solving a Vandermonde linear system with rational solutions.

## The trick

Start from the primitive: $Y(a,b) = e^{a\lambda A} e^{b\lambda B} e^{-a\lambda A} e^{-b\lambda B} e^{-a\lambda A} e^{b\lambda B} e^{a\lambda A}$ implements

$$Y(a,b) = \exp\!\left(2ab\lambda^2 [A,B] + ab^2 \lambda^3 [B,A,B] + O(\lambda^4)\right).$$

For higher-order commutators, compose multiple $Y$ factors:

$$\prod_{\ell=0}^{m-1} Y(a_\ell, b_\ell) \prod_{\ell=0}^{m-1} Y(-a_\ell, -b_\ell) = \exp\!\left(\sum_{j=1}^{m} c_j \lambda^{2j} \mathrm{ad}_A^{2j-1}(B) + O(B^3)\right).$$

Matching coefficients $c_j$ to the desired corrector values yields the linear system:

$$\begin{pmatrix} a_0 & a_1 & \cdots \\ a_0^3 & a_1^3 & \cdots \\ \vdots & & \ddots \end{pmatrix} \begin{pmatrix} b_0 \\ b_1 \\ \vdots \end{pmatrix} = \begin{pmatrix} B_2(1/2)/8 \\ B_4(1/2)/16 \\ \vdots \end{pmatrix}.$$

Set $a_\ell = \ell + 1$ (arbitrary distinct nonzero values). The coefficient matrix factors as a Vandermonde matrix times a diagonal, so inversion is explicit and all entries are rational.

## When to reach for it

- Compiling the [[Symplectic Corrector Injection for Product Formulas|symplectic correctors]] for corrected product formulas
- Implementing $e^{c\lambda^{2m} \mathrm{ad}_A^{2m-1}(B)}$ for single-commutator correctors (Yoshida CPFs)
- Synthesising unitaries from nested commutator generators on a quantum simulator with limited native gates
- Any setting where you need a product-formula implementation of a commutator exponential and [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs-Wiebe recursion]] is too expensive

## Complexity

$10k$ exponentials for the [[Bernoulli Polynomial Kernel Expansion for Product Formulas|$k$-term Bernoulli corrector]]. Each $Y(a,b)$ costs 5 exponentials (after simplification), and two $Y$ factors are needed per level. Compilation error: $O(\alpha^3 |\lambda|^3)$ — cubic in the perturbation parameter, which is subdominant to the CPF error.

## Caveat

- Works modulo terms of degree $\ge 3$ in $B$. For non-perturbed systems, these discarded terms are the same order as what you're keeping — the compilation is only useful when $\alpha \ll 1$ or when combined with symmetric correctors.
- At high $k$, the Vandermonde condition number grows, which could affect numerical stability of the $b_\ell$ coefficients. In practice, the rational solutions for $a_\ell = \ell + 1$ are exact.
- This is the $Y$-based approach from the paper. The alternative $W(a)$ primitive (from Chen, Childs et al. 2022) uses 6 exponentials for a single $[A,B]$ term with simpler structure.

## Related notes
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]]
- [[Symplectic Corrector Injection for Product Formulas]]
- [[Bernoulli Polynomial Kernel Expansion for Product Formulas]]
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]]
- [[Suzuki-Like Recursion for Even-k Commutator Exponentials]]
- [[Finite Nested-Commutator Expansion]]
