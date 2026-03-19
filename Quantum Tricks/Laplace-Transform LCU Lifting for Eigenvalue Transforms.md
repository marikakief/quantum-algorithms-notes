# Laplace-Transform LCU Lifting for Eigenvalue Transforms

> **Source:** An, Childs, Lin, arXiv:2411.04010
> **Tags:** #trick #eigenvalue-transform #LCHS #LCU #Laplace

## What it does

Converts any eigenvalue transformation $h(A)$ with a well-behaved inverse Laplace transform into a [[Linear Combination of Unitaries (LCU)|linear combination of unitaries]] — specifically, of Hamiltonian simulation operators.

## The trick

Start from two ingredients:
1. $h(z) = \int_0^\infty g(t)\, e^{-zt}\, dt$ (Laplace representation of $h$)
2. The [[LCHS Kernel for Non-Unitary Dynamics|LCHS identity]]: $e^{-tA} = \int_\mathbb{R} \frac{f(k)}{1-ik}\, e^{-it(kL+H)}\, dk$ for dissipative $A = L + iH$

Substitute (2) into (1):

$$h(A) = \int_0^\infty \int_\mathbb{R} \frac{f(k)\, g(t)}{1-ik}\, e^{-it(kL+H)}\, dk\, dt$$

The integrand is a unitary ($e^{-it(kL+H)}$) weighted by a scalar kernel. Truncate and discretise both integrals → finite LCU of Hamiltonian simulation unitaries. Implement via standard [[Linear Combination of Unitaries (LCU)|LCU]] protocol + [[Standard Amplitude Amplification|amplitude amplification]].

The block-encoding normalisation is $\|f\|_{L^1} \cdot \|g\|_{L^1}$.

## When to reach for it

- You need to apply a non-polynomial function of a non-normal matrix (e.g., fractional powers $(\eta I + A)^{-p}$, $e^{-TA^{-1}}$)
- The matrix has the dissipativity property $L = (A + A^\dagger)/2 \succeq 0$
- The function $h$ has a known Laplace/inverse-Laplace representation
- You already have Hamiltonian simulation subroutines available (the inner loop is standard Ham-sim)

## Complexity

$\tilde{O}(\alpha_A T \cdot (\log 1/\varepsilon)^{1+o(1)})$ queries to the block encoding of $A$, where $T$ is the Laplace truncation and $\alpha_A$ is the block-encoding factor. State-preparation cost is optimal: $O(\|g\|_{L^1} / \|h(A)|\psi\rangle\|)$ copies of the input state.

## Caveat

- Requires dissipativity ($L \succeq 0$). Non-dissipative matrices need different tools (e.g., [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes|QEVT]]).
- If $g(t)$ decays slowly, the truncation $T$ grows and complexity degrades.
- The approach is restricted to functions in the Laplace-transform class. Oscillatory functions or functions with branch cuts on the imaginary axis may not have suitable representations.

## Related notes

- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]]
- [[LCHS Kernel for Non-Unitary Dynamics]]
- [[Near-Optimal Hardy-Space Kernel for LCHS]]
- [[Double-Integral Quadrature for Operator LCU]]
- [[Linear Combination of Unitaries (LCU)]]
