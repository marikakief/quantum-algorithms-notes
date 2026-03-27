
> **Source:** Berry, Su, Gyurik, King, Basso, Del Toro Barba, Rajput, Wiebe, Dunjko, Babbush, arXiv:2209.13581, PRX Quantum **5**, 010319 (2024)
> **Tags:** #trick #eigenvalue-filtering #Chebyshev #qubitization #kernel-estimation #walk-operator

## What it does

Projects a quantum state onto the zero-eigenvalue subspace of a Hamiltonian $H$ by applying a Chebyshev polynomial filter to the [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] walk operator. More efficient than phase estimation when you only need to distinguish zero from nonzero eigenvalues, rather than estimate eigenvalue magnitudes.

## The trick

Under [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]], eigenvalue $E_k$ of $H$ maps to walk operator eigenphases $\pm \arcsin(E_k/\lambda)$. Zero eigenvalues of $H$ become eigenvalues $\pm 1$ of the walk operator (phases $0$ and $\pi$).

Construct a filter function peaked at phases $0$ and $\pi$:

$$\tilde{w}(\phi) = \epsilon \, T_\ell(\beta \cos\phi)$$

where $T_\ell$ is the Chebyshev polynomial of degree $\ell$, $\beta = \cosh\!\left(\frac{1}{\ell}\cosh^{-1}(1/\epsilon)\right)$, and $\epsilon$ is the suppression factor for non-kernel components.

The filter is implemented as a linear combination of powers of the walk operator $W$ and its inverse $W^\dagger$, controlled by toggling which operator is applied at each step. Since $\tilde{w}(\phi)$ is a function of $\cos\phi$ and involves only even powers, the LCU has $\ell$ terms.

The required degree to suppress eigenvalues outside the spectral gap $\lambda_{\min}$ by factor $\epsilon$:

$$\ell = \frac{\cosh^{-1}(1/\epsilon)}{\cosh^{-1}(1/\sqrt{1 - (\lambda_{\min}/\lambda)^2})} \leq \frac{\lambda}{\lambda_{\min}}\ln(2/\epsilon)$$

## When to reach for it

- **Betti number estimation:** You need the dimension of a kernel, not individual eigenvalues. Phase estimation provides more information than needed and pays for it.
- **Any kernel dimension estimation problem** where the operator is available via [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] and the spectral gap is known or can be bounded.
- **Binary spectral decisions:** "Is this eigenvalue zero or not?" rather than "What is this eigenvalue?"
- As a replacement for the phase estimation step in overlap estimation protocols when eigenvalue magnitudes aren't needed.

## Complexity

$\frac{n}{\lambda_{\min}}\ln(2/\epsilon)$ calls to the block encoding of $H$, where $\lambda = n$ for the TDA Dirac operator.

This is asymptotically similar to optimal phase estimation (the Kaiser-window version from the same paper) for distinguishing zero from nonzero, but avoids the modest constant-factor overhead (~60% in the non-asymptotic regime) that phase estimation incurs from providing a full eigenvalue estimate.

## Caveat

Requires knowledge of (or a lower bound on) the spectral gap $\lambda_{\min}$. If $\lambda_{\min}$ is very small, the filter degree grows as $1/\lambda_{\min}$ — same as phase estimation, so there's no asymptotic escape from the gap dependence. The advantage is purely in not paying for unnecessary precision in the eigenvalue estimate.

The connection to the [[Dolph-Chebyshev Eigenstate Filtering|Dolph-Chebyshev filtering]] used in QLSP solvers is close — both use Chebyshev polynomials on walk operators. The difference is the target: QLSP filtering isolates eigenvalues near a *specific* value (via spectral inversion), while this projects onto the kernel (eigenvalues $\pm 1$ of the walk).

## Related notes
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]]
- [[Dolph-Chebyshev Eigenstate Filtering]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
