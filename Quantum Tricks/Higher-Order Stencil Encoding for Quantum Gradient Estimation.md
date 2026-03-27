
> **Source:** Gilyén, Arunachalam, Wiebe, arXiv:1711.00465 (2019)
> **Tags:** #trick #gradient #finite-differences #oracle

## What it does
Quadratically reduces the oracle precision needed for quantum gradient estimation by encoding higher-order central difference coefficients as quantum state amplitudes.

## The trick

[[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan's algorithm]] prepares a uniform superposition over grid points — this is a first-order finite difference stencil. The error is dominated by the Hessian (second derivatives).

Replace the uniform superposition with a state whose amplitudes match a $k$-th order central difference formula:

$$|\psi\rangle = \sum_j c_j |x + jh\rangle$$

where the $c_j$ are chosen so that terms through order $k$ in the Taylor expansion cancel. Classically, the same idea gives higher-order finite difference formulas. Quantumly, it means the function evaluation precision $n_o$ required drops from $n + O(d)$ (Jordan) to $n + O(\log d)$ for functions with bounded $k$-th derivatives.

This requires $O(\log d)$ oracle queries instead of Jordan's 1, because the stencil state can't be prepared with a single query. But the precision savings more than compensate.

## When to reach for it
- Quantum gradient estimation where oracle precision is expensive (e.g., the oracle involves [[Hamiltonian simulation]] or amplitude estimation)
- Smooth functions with controlled higher derivatives
- Optimisation of quantum circuit parameters (VQE, QAOA) where each function evaluation is a full quantum circuit run

## Complexity
- Queries: $O(\log d)$ for $d$-dimensional gradient with $n$-bit precision
- Precision: $n_o = n + O(\log d)$ (quadratic improvement over Jordan's $n + O(d)$)

## Caveat
Requires bounded higher derivatives. For non-smooth or discontinuous functions, higher-order stencils don't help and may make things worse.

## Related notes
- [[Optimizing Quantum Optimization Algorithms via Faster Quantum Gradient Computation (Gilyén-Arunachalam-Wiebe 2019) — Paper Notes]]
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]]
- [[Fourier-Eigenstate Kickback for Arithmetic Oracles]]
