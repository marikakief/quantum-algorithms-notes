
> **Source:** Tong, An, Wiebe & Lin, arXiv:2008.13295, Phys. Rev. A **104**, 032422 (2021)
> **Tags:** #trick #QSVT #matrix-function #Chebyshev #Gevrey #block-encoding

## What it does

Evaluates $f(A + B)$ (e.g., $e^{-\beta(A+B)}$) by rewriting it as a function $g$ of the preconditioned inverse and implementing $g$ via Chebyshev polynomial approximation + [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]].

## The trick

If $A$ is [[Fast Inversion for Classically-Diagonalizable Matrices|fast-invertible]], the eigenvalues $\lambda$ of $A + B$ relate to eigenvalues $\mu$ of $(A + B)^{-1}$ by $\lambda = 1/\mu$. Define:

$$g(\mu) = f(1/\mu)$$

If $f$ is smooth (e.g., $f(x) = e^{-\beta x}$), then $g(\mu) = e^{-\beta/\mu}$ is also smooth — specifically, it belongs to the **Gevrey class** with controlled derivative growth. The paper uses the Faà di Bruno formula to bound derivatives of composed functions and show that $g$ can be approximated on $[-1,1]$ by a Chebyshev polynomial of bounded degree.

**Implementation:**
1. [[Preconditioning Quantum Linear Systems via Fast Inversion|Preconditioned inversion]] gives a block-encoding of $(A+B)^{-1}$
2. Approximate $g$ by a degree-$d$ Chebyshev polynomial with $d = O(\text{polylog}(\alpha_B, \beta, 1/\epsilon))$ for Gevrey-class $g$
3. Apply QSVT with the Chebyshev polynomial phases to the block-encoding of $(A+B)^{-1}$

Result: a block-encoding of $f(A+B) = g((A+B)^{-1})$ with cost scaling in $\alpha'_A$, $\alpha_B$, $\tilde{\sigma}_{\min}$, and the function parameters ($\beta$ for thermal states).

## When to reach for it

- **Gibbs state preparation** via $e^{-\beta H/2}$ when $H = T + V$ and $T$ is fast-invertible
- Any matrix function $f(H)$ where $H$ has a preconditionable splitting and $f(1/\cdot)$ is sufficiently smooth
- Alternative to the [[Contour Integral Matrix Exponentiation via QSVT|contour integral approach]]: the contour method gives $\alpha_{\text{LCU}} = O(\alpha_B\sqrt{\beta})$; the inverse-transform route has different (sometimes better) parameter trade-offs depending on smoothness

## Complexity

Polynomial degree: controlled by Gevrey-class index $s$ and the block-encoding parameters. For $e^{-\beta H}$: degree $O(\text{polylog}(\beta, 1/\epsilon))$ under Gevrey-$s$ assumptions.

Each QSVT query uses $O(\alpha'_A \alpha_B / \tilde{\sigma}_{\min})$ applications of the primitive block-encodings. Total: degree $\times$ per-query cost.

## Caveat

The Gevrey-class smoothness analysis is technically involved and the constants matter. For functions that aren't Gevrey-smooth (e.g., non-analytic at $\mu = 0$), the polynomial degree bound degrades. The approach also inherits all limitations of [[Preconditioning Quantum Linear Systems via Fast Inversion|preconditioned inversion]] — $A$ must be fast-invertible and $\tilde{\sigma}_{\min}$ must be controlled.

Compared to the contour integral route, this avoids the LCU over quadrature points but needs a good polynomial approximation. The two approaches are complementary rather than one being strictly better.

## Related notes
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]]
- [[Preconditioning Quantum Linear Systems via Fast Inversion]]
- [[Fast Inversion for Classically-Diagonalizable Matrices]]
- [[Contour Integral Matrix Exponentiation via QSVT]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[QSVT Meta-Template]]
