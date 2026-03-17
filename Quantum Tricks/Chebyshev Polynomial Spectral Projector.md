
> **Tags:** #trick #chebyshev #spectral-filtering #tda #qsvt
> **Source:** Babbush et al. arXiv:2209.13581; broader polynomial filtering literature

## What it does
Approximates a sharp spectral projector (e.g., onto near-zero eigenspace) using a low-degree Chebyshev polynomial of a block-encoded operator.

## The trick
Given Hermitian $A$ with spectrum in [-1,1], approximate indicator function $\mathbf{1}_{|x|\le \eta}$ by polynomial $p_m(x)$ built from Chebyshev basis $T_j(x)$:
$$p_m(x) \approx \mathbf{1}_{|x|\le \eta}$$
Implement $p_m(A)$ via [[Qubitization Iterate|qubitization]]/[[QSVT Meta-Template|QSVT]]-style sequences.

For TDA: project onto kernel of Hodge Laplacian / Dirac (Betti subspace) without full phase estimation.

## When to reach for it
- Need eigenvalue thresholding, not exact eigenvalues
- Want ancilla-efficient alternative to full phase estimation
- Spectral gap known or lower bounded

## Complexity
Degree scales roughly with inverse transition width (gap) and log(1/error):
$$m = O\!\left(\frac{1}{\gamma}\log\frac{1}{\epsilon}\right)$$
where $\gamma$ is spectral separation.

## Caveat
If gap is tiny, polynomial degree explodes. Requires careful rescaling and robust error budgeting.

## Related Paper Notes

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
