# QHO Fast-Forwarding via Lie Algebra Factorisation

> **Tags:** #trick #fast-forwarding #hamiltonian-simulation #harmonic-oscillator
> **Source:** arXiv:2510.04929 (Quantum Hermite Transform), Theorem 1

## What it does
Simulates the quantum harmonic oscillator $e^{-i\hat{H}t}$ in **$O(\log^2 N)$ gates** instead of $O(Nt)$ — exponential fast-forwarding.

## The trick
The operators $\hat{x}^2$, $\hat{p}^2$, and $\{\hat{x}, \hat{p}\}/2$ generate $\mathfrak{sl}(2, \mathbb{R})$ — a 3-dimensional Lie algebra. This closed Lie algebra structure means the BCH expansion is an exact finite decomposition in the continuum:

$$e^{-i\hat{H}t} = e^{-i\frac{\tan(t/2)}{2}\hat{p}^2} \cdot e^{-i\frac{\sin t}{2}\hat{x}^2} \cdot e^{-i\frac{\tan(t/2)}{2}\hat{p}^2}$$

(Exact factorization; see Appendix A of arXiv:2510.04929 for the proof.) Each factor is diagonal in either the position or momentum basis, so each can be implemented via QFT + phase kickback in $O(\log^2 N)$ gates on an $M$-point lattice.

The discrete version requires $M = \Theta(N \log N)$ grid points. The commutator errors from discretization are doubly-exponentially small on the low-energy subspace (first $N$ eigenstates), making the approximation extremely accurate for any polynomial-in-$N$ target error.

## When to reach for it
- Any Hamiltonian whose terms generate a finite-dimensional Lie algebra
- Quadratic Hamiltonians (harmonic oscillators, free bosons, squeezed states)
- As a subroutine in the QHT (eigenstate filtering via fast QPE, enabling [[Plancherel-Rotach Asymptotic State Preparation]])
- Hamiltonians that are sums of $\hat{x}^2$ and $\hat{p}^2$ type terms with known Lie structure

## Complexity
$O(\log^2 N)$ gates on an $M = \Theta(N \log N)$-dimensional register. Error: $O(e^{-N/10})$ on the low-energy subspace (first $N$ eigenstates). The $\log^2 N$ comes from schoolbook multiplication in coherent arithmetic; could potentially be improved with more sophisticated arithmetic circuits.

## Caveat
Only works when the Lie algebra is finite-dimensional. Anharmonic terms ($\hat{x}^4$, etc.) break the factorisation. The discretisation requires $M = \Theta(N \log N)$ grid points.

## Generalisation potential
Any Hamiltonian with $\mathfrak{sl}(2)$, $\mathfrak{su}(2)$, or other low-dimensional Lie algebra structure could admit similar fast-forwarding. Look for: quadratic combinations of conjugate variables, or operators satisfying simple commutation relations.
