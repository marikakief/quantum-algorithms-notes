
> **Tags:** #trick #state-preparation #fourier #coefficients
> **Source:** arXiv:2401.06240 (Quantum eigenvalue processing, FOCS 2024)

## What it does

Prepares a quantum superposition over polynomial coefficients $\sum_k c_k |k\rangle$ in $O(\text{polylog}(n))$ gates, where $\{c_k\}_{k=0}^{n-1}$ are the Chebyshev (or Faber) expansion coefficients of a target function $f$. Prior approaches required $O(n)$ gates (linear QROM loading). This is an improvement that the paper notes "of independent interest" beyond its use in QEVE/QEVT.

## The trick

The Chebyshev coefficients of a function $f: [-1,1] \to \mathbb{C}$ are:
$$c_k = \frac{2}{\pi}\int_0^\pi f(\cos\theta)\cos(k\theta)\,d\theta$$

This is a **discrete cosine transform (DCT)** of the sampled values $\{f(\cos(\pi j/(n-1)))\}_{j=0}^{n-1}$. In the quantum setting:

1. [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] a uniform superposition over sample points: $\frac{1}{\sqrt{n}}\sum_j |j\rangle$.
2. Apply a quantum oracle computing $f(\cos(\pi j/(n-1)))$ into an amplitude register.
3. Apply the quantum Fourier transform (or DCT) on the $j$-register.
4. The result is (proportional to) $\sum_k c_k |k\rangle$ — the coefficient superposition.

The QFT step costs $O(\log^2 n)$ gates. The oracle call costs whatever $f$ evaluation costs. The full pipeline reduces coefficient loading from $O(n)$ (QROM) to $O(\text{polylog}(n) + T_f)$ where $T_f$ is the cost of one $f$-evaluation.

## Connection to convolution

The Chebyshev coefficients are a convolution in $\theta$-space: $c_k = \langle f(\cos(\cdot)), \cos(k\cdot)\rangle_{[0,\pi]}$. The convolution structure is what makes the QFT applicable — it diagonalizes the inner product. For Faber coefficients, the same idea applies via the conformal map $\phi$: coefficients are Fourier modes in $\phi$-space.

## When to reach for it

- Polynomial transform algorithms where the coefficient superposition is a bottleneck (QEVT, QEVE, Chebyshev-based linear systems)
- Any setting where the target function has a known analytic form enabling efficient evaluation
- Replacing large QROM tables for polynomial coefficient loading

## Complexity

$O(\log^2(n) + T_f)$ gates, where $T_f$ = evaluation cost of $f$. For functions evaluable via QROM in $O(\text{polylog}(n))$ (smooth functions with polynomial-sized tables), the total cost is $O(\text{polylog}(n))$. Prior methods: $O(n)$ via linear QROM.

## Caveat

- Requires the function $f$ to be evaluable (or approximable) in quantum arithmetic — a non-trivial requirement for general $f$.
- The DCT / QFT gives exact Chebyshev coefficients only at the Chebyshev nodes; for other functions, there's an approximation error from the quadrature.
- Works cleanly for Chebyshev; Faber coefficients require the additional classical conformal map data, but the principle is the same.

## Related Paper Notes
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]]
- [[Faber-Polynomial Region Mapping for Complex-Spectrum Eigenvalue Transforms]]
