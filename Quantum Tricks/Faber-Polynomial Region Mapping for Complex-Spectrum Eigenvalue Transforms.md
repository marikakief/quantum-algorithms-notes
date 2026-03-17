
> **Tags:** #trick #faber #complex-spectrum #qevt
> **Source:** arXiv:2401.06240 (Quantum eigenvalue processing, FOCS 2024)

## What it does

Generalizes polynomial eigenvalue processing from real intervals to complex spectral regions, enabling QEVT for non-normal matrices with genuinely complex eigenvalues. Faber polynomials are to complex domains what Chebyshev polynomials are to the interval $[-1,1]$: they provide near-optimal uniform polynomial approximations.

## Background: why Chebyshev fails for complex spectra

For a matrix $A$ with eigenvalues on $[-1,1]$, Chebyshev polynomials $\{T_k\}$ are the natural basis: they have $\|T_k\|_\infty = 1$ on $[-1,1]$ and grow slowly outside. When eigenvalues are in a general region $E \subset \mathbb{C}$ (which happens for non-normal matrices), there's no natural polynomial basis with bounded growth on $E$ — unless we use the geometry of $E$ explicitly.

## The Faber polynomial construction

Given a compact simply-connected region $E$ containing the spectrum of $A$:

1. **Conformal map:** Compute (classically) the conformal map $\phi: \mathbb{C} \setminus E \to \mathbb{C} \setminus \bar{\mathbb{D}}$ (exterior of $E$ to exterior of unit disk), normalized so $\phi(\infty) = \infty$.

2. **Faber polynomials:** Define $F_n(z)$ as the polynomial part of the Laurent expansion of $[\phi(z)]^n$ as $z \to \infty$:
$$[\phi(z)]^n = F_n(z) + O(1/z) \quad \text{as } z \to \infty$$
   
   $F_n$ is a degree-$n$ polynomial. On $E$, $|\phi(z)| \leq 1$, so $|F_n(z)| \leq 1 + O(1/n)$ on $E$ — bounded just like Chebyshev.

3. **Faber expansion:** Any analytic function $f$ on $E$ has a Faber series $f(z) = \sum_n a_n F_n(z)$ with geometric convergence.

4. **Quantum implementation:** Replace the Chebyshev recurrence with the Faber recurrence (which also has a three-term structure determined by $\phi$), and apply the [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|QLSA]]-based [[History-State Encoding with Unary Clock|history state]] approach.

## The key property being exploited

For Chebyshev: $T_k(\cos\theta) = \cos(k\theta)$ — eigenvalues parametrized by angle → Fourier readout gives the angle → invert $\cos$ to get eigenvalue.

For Faber: eigenvalues $\lambda_j \in E$ map to $\phi(\lambda_j) = e^{i\theta_j}$ (on the unit circle, by conformality). The Faber basis satisfies $F_k(\lambda_j) = e^{ik\theta_j}$ (the Faber–Riesz identity). So the [[History-State Encoding with Unary Clock|history state]] contains a Fourier series in $k$ with frequency $\theta_j$ — same readout strategy as Chebyshev, but now $\theta_j = \arg(\phi(\lambda_j))$ encodes the complex eigenvalue.

## When to reach for it

- QEVT for non-normal matrices with complex spectra (non-Hermitian Hamiltonians, open quantum systems, iterative method analysis)
- Any setting where the spectrum occupies a known bounded convex region in $\mathbb{C}$
- When Chebyshev-based methods fail because eigenvalues are off the real axis

## Complexity

Same asymptotic structure as the Chebyshev case, with the conformal map data supplied classically. The approximation quality depends on the geometry of $E$ — rounder regions (close to a disk) give better polynomial approximation than elongated or irregular regions.

## Caveat

- Computing the conformal map $\phi$ and its Taylor coefficients is classical preprocessing, but for complicated regions $E$ it requires numerical conformal mapping (e.g., Schwarz-Christoffel methods). Not always trivial.
- The Faber–Riesz identity $F_k(\lambda_j) = e^{ik\theta_j}$ requires the spectrum to lie strictly inside $E$ (not on the boundary). Eigenvalues near the boundary of $E$ can cause slow convergence.
- If the spectrum is not known in advance, a conservative choice of $E$ (e.g., the convex hull of the numerical range) may be necessary, potentially degrading approximation quality.
- For Hermitian matrices, $E = [-1,1]$ and Faber polynomials reduce to Chebyshev — the two methods coincide.

## Related Paper Notes
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]]
- [[Chebyshev-State Phase Estimation for Real-Spectrum Non-Normal Inputs]]
