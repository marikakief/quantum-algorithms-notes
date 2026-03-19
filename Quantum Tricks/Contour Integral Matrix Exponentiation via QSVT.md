# Contour Integral Matrix Exponentiation via QSVT

> **Source:** Fang, Lin & Tong, arXiv:2208.06941, Quantum **7**, 955 (2023), Appendix G. Builds on Tong, An, Wiebe & Lin, Phys. Rev. A **104**, 032422 (2021).
> **Tags:** #trick #QSVT #matrix-function #contour-integral #non-normal #block-encoding

## What it does

Implements $e^A$ for a **non-normal** matrix $A$ given as a block-encoding, by discretising the Cauchy integral formula and computing each resolvent via [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] matrix inversion.

## The trick

Use the contour integral representation:

$$e^A = \frac{1}{2\pi i} \oint_\Gamma e^z (z - A)^{-1}\,dz$$

where $\Gamma$ is a circle of radius $\beta = 2\alpha$ containing all eigenvalues of $A$. Discretise with $K$ quadrature points on $\Gamma$:

$$e^A \approx f_K(A) = \frac{1}{K}\sum_{k=0}^{K-1} e^{z_k} z_k (z_k - A)^{-1}, \quad z_k = 2\alpha\, e^{i2\pi k/K}.$$

The error is $O(e^{4\alpha} \cdot 2^{-K})$ by the exponentially convergent trapezoidal rule (Trefethen-Weideman).

**Implementation:**
1. Block-encode $\bigoplus_{k=0}^{K-1} (z_k - A)$ using the block-encoding of $A$ plus a controlled phase shift indexed by $k$.
2. Invert this block-diagonal operator via [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] (the singular values are at least $\alpha$ since $|z_k| = 2\alpha$, so the inversion polynomial has moderate degree).
3. Prepare coefficient states $|\text{COEF}\rangle \propto \sum_k \sqrt{e^{z_k} z_k}\,|k\rangle$ and $|\text{COEF}'\rangle$ to form the LCU.

Result (Lemma 12): a $(4\mathcal{A}/(3\alpha), m + O(\log\alpha + \log\log(1/\epsilon)), \epsilon)$-block encoding of $e^A$, using $O(\alpha + \log(1/\epsilon))$ applications of controlled-$U_A$.

## When to reach for it

- Implementing matrix exponentials for **non-normal** matrices (where standard polynomial approximation of $e^{ix}$ via QSVT doesn't apply, since singular values $\neq$ eigenvalues)
- The first-order Magnus integrator for time-marching ODE solvers: each short-time step requires $e^{\int A(s)\,ds}$ where $A$ is generally non-Hermitian
- Any quantum algorithm needing $f(A)$ for analytic $f$ and non-normal $A$ (the contour integral works for any function analytic inside $\Gamma$)

## Complexity

$O(\alpha + \log(1/\epsilon))$ queries to the block-encoding of $A$, plus $O(\alpha^2 + \log^2(1/\epsilon))$ elementary gates. The normalisation factor $\mathcal{A} = \frac{2\alpha}{K}\sum_k |e^{z_k}|$ is $\Theta(\alpha)$ for the standard choice $\beta = 2\alpha$.

## Caveat

Only works when $\alpha$ (the block-encoding normalisation of $A$) is $O(1)$ — otherwise the contour radius grows and the Cauchy integral coefficients blow up. In the time-marching context, short time segments ensure $\alpha(t_l - t_{l-1}) = O(1)$. For large-norm $A$, you'd need to rescale or use a different approach.

The QSVT inversion step requires computing phase factors for the inversion polynomial, adding classical preprocessing cost.

## Related notes
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]]
- [[Uniform Singular Value Amplification for Block-Encoding Norm Matching]]
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]] — uses contour integral with preconditioned resolvents instead of direct QSVT inversion
- [[Inverse-Transform QSVT for Matrix Functions]] — alternative to contour integral: Chebyshev approximation of $f(x^{-1})$
