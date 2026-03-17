# Generating-Function-to-QLSA Reduction for Polynomial Basis Histories

> **Tags:** #trick #qlsa #polynomial-transforms #non-normal
> **Source:** arXiv:2401.06240 (Quantum eigenvalue processing, FOCS 2024)

## What it does

Prepares the polynomial basis superposition $\sum_{k=0}^{d-1} c_k |k\rangle |p_k(A)|v\rangle$ using $O(\text{polylog}(d))$ [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] queries to $A$, instead of $O(d)$. This is the core step enabling both QEVT and QEVE.

## The trick

The sequence of polynomial basis vectors $\{p_k(A)|v\rangle\}_{k=0}^{d-1}$ satisfies the three-term recurrence (for Chebyshev):
$$p_{k+1}(A) = 2A\,p_k(A) - p_{k-1}(A)$$

Stacking this into a block equation on the composite register $|k\rangle \otimes \mathcal{H}$:
$$(2A \otimes I - I \otimes L - I \otimes L^\dagger)|P\rangle = |b\rangle$$
where $|P\rangle = \sum_k |k\rangle|p_k(A)v\rangle$, $L$ is the lower-shift on the $k$-register, and $|b\rangle$ encodes boundary conditions ($|p_0(A)v\rangle$ and $|p_1(A)v\rangle$, which cost $O(1)$ [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] queries).

This is a **block-tridiagonal linear system** of size $d \times \dim(\mathcal{H})$, with the $A$-block on the diagonal and $\pm I$ on the super/sub-diagonals. The condition number of this system is $O(d \cdot \kappa_V)$, where $\kappa_V$ is the eigenbasis conditioning. Inverting via QLSA (e.g., HHL or an optimal QLSA variant) prepares $|P\rangle$ with cost proportional to $O(\kappa_V \log d)$ [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] queries — a $d/\log d$ improvement over the naive approach.

## Why the structure works

The banded structure (bandwidth 1 in the $k$-register) ensures the effective condition number grows only polynomially with $d$, not exponentially. This is what makes the QLSA call efficient — general linear systems would have no reason to be well-conditioned, but the recurrence structure guarantees it.

## When to reach for it

- Any algorithm requiring a polynomial transform of a block-encoded matrix
- Eigenvalue estimation for non-normal (or normal) matrices where QPE on $e^{-iAt}$ is expensive
- Building polynomial basis states as subroutines for QEVT, QEVE, or differential equation solvers

## Complexity

$O(\kappa_V \log(d/\varepsilon))$ [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] queries (via QLSA), plus $O(\text{polylog}(d))$ overhead for the register arithmetic. Compare to naive $O(d)$ — improvement is roughly $d / (\kappa_V \log d)$.

## Caveat

The QLSA condition number depends on $\kappa_V$ (eigenbasis conditioning). For highly non-normal matrices, $\kappa_V$ can be large or even exponential, negating the gain. The trick is most powerful when the matrix is only mildly non-normal (e.g., has well-conditioned eigenbasis despite non-Hermiticity).

## Related Paper Notes
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]]
- [[Lower-Shift Ancilla Encoding for Polynomial Degree Management]]
