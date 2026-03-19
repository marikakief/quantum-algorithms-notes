# Chebyshev-Node Loss for QSP Phase Optimisation

> **Source:** Dong, Meng, Whaley & Lin, arXiv:2002.11649, Phys. Rev. A **103**, 042419 (2021)
> **Tags:** #trick #QSP #phase-factors #optimisation #Chebyshev #sampling

## What it does

Converts [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] phase-factor computation from a continuous approximation problem into a discrete least-squares problem with provable coefficient-level control, by evaluating the loss at Chebyshev nodes.

## The trick

Define the loss function

$$L(\Phi) = \frac{1}{\tilde{d}} \sum_{k=1}^{\tilde{d}} \bigl|g(x_k, \Phi) - f(x_k)\bigr|^2,$$

where $\tilde{d} = \lceil d/2 \rceil$ and $x_k$ are the positive roots of $T_{2\tilde{d}}(x)$. Here $g(x,\Phi) = \operatorname{Re}[\langle 0|U_\Phi(x)|0\rangle]$ is the QSP output polynomial and $f$ is the target.

**Key property (Theorem 4 in the paper):** If $L(\Phi) \leq \varepsilon$, then the Chebyshev coefficients of $g$ and $f$ agree to within $2\sqrt{\varepsilon}$ in max-norm:

$$\max_j |\hat{g}_j - \hat{f}_j| \leq 2\sqrt{\varepsilon}.$$

The proof uses discrete orthogonality of Chebyshev polynomials on their own roots — the same property that makes Chebyshev interpolation exact for polynomials of degree $\leq 2\tilde{d} - 1$.

## When to reach for it

- Any optimisation-based QSP or [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] phase-factor search
- Choosing sample points for verifying or fitting polynomial transformations of block-encoded operators
- Anytime you need to control polynomial coefficients from pointwise samples — the Chebyshev-node choice gives the tightest possible pointwise-to-coefficient transfer

## Complexity

Evaluating $L$ costs $O(d \cdot \tilde{d})$: each of the $\tilde{d}$ sample points requires multiplying $d$ SU(2) matrices. Gradient costs the same order via backpropagation through the matrix product.

## Caveat

The $\tilde{d}$ sample points only control even or odd Chebyshev coefficients (matching the parity of $f$). If parity is violated, the loss can be zero while the polynomial is wrong on the "other" parity. In practice, the symmetric-phase restriction enforces parity automatically.

## Related notes
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]]
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]]
- [[Symmetry Reduction for Phase Factor Uniqueness]]
- [[Perturbative Initial Guess for QSP Phases]]
