# On-the-Fly Coherent Molecular Integral Computation

> **Source:** Babbush, Berry, Kivlichan, Wei, Love, Aspuru-Guzik, arXiv:1506.01020
> **Tags:** #trick #quantum-chemistry #LCU #state-preparation #integral-computation

## What it does

Computes molecular two-electron integrals coherently on the quantum computer during the PREPARE step of an [[Linear Combination of Unitaries (LCU)|LCU]] simulation, instead of loading them from a classically precomputed database. Reduces the per-call PREPARE cost from $O(N^4)$ to $O(N)$ for $N$ spin-orbitals.

## The trick

The two-electron integrals $h_{pqrs}$ can be written as integrals over an auxiliary continuous variable $z$ (related to the Boys function and Gaussian decomposition of the Coulomb interaction):

$$h_{pqrs} = \int f_p(z) f_q(z) f_r(z) f_s(z)\, W(z)\, dz,$$

where $f_p(z)$ are one-electron functions evaluated at the auxiliary point $z$.

Discretise on $M$ quadrature points $\{z_m\}$. Now the integral factorises: instead of preparing a superposition over all $O(N^4)$ index tuples $(p,q,r,s)$ with amplitudes proportional to $h_{pqrs}$, prepare:

1. A superposition over quadrature points $m$ with amplitudes $\propto \sqrt{|W(z_m)|}$.
2. Conditioned on $m$, prepare orbital-index registers with amplitudes $\propto \sqrt{|f_p(z_m)|}$ — each register costs $O(N)$.

The factorisation means the four orbital indices are prepared independently (each $O(N)$) rather than jointly ($O(N^4)$). The Gaussian basis function evaluations use quantum arithmetic (additions, multiplications, exponentials to polylogarithmic precision).

## When to reach for it

- Quantum chemistry simulation where storing all $O(N^4)$ integrals is too expensive for the PREPARE oracle.
- Any [[Linear Combination of Unitaries (LCU)|LCU]]-based simulation where the Hamiltonian coefficients have an integral representation that factorises over indices.
- More generally: whenever you can express a high-rank tensor of coefficients as a low-rank factorisation via an auxiliary integral, this converts an expensive state-preparation problem into a cheaper one.

## Complexity

$O(N)$ gates per PREPARE call (down from $O(N^4)$ for the database variant), plus $O(\text{polylog}(N, 1/\varepsilon))$ overhead for quantum arithmetic. Requires $O(\log M)$ ancilla qubits for the quadrature register.

## Caveat

The quantum arithmetic for evaluating Gaussian basis functions to exponential precision is non-trivial — the paper doesn't give explicit circuit constructions, just asymptotic costs. The constant factors and ancilla overhead for the arithmetic could be substantial. Also, the quadrature discretisation introduces an additional approximation error that must be budgeted within the total $\varepsilon$.

Modern approaches (double factorisation, tensor hypercontraction) achieve similar or better factorisations with more explicit and practical circuits.

## Related notes

- [[Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) — Paper Notes]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]]
