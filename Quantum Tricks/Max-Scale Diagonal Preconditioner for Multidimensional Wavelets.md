# Max-Scale Diagonal Preconditioner for Multidimensional Wavelets

> **Source:** Bagherimehrab, Nakaji, Wiebe, Brennen, Sanders & Aspuru-Guzik, arXiv:2306.11802
> **Tags:** #trick #wavelets #preconditioning #PDE #quantum-circuits

## What it does

Implements a multidimensional wavelet preconditioner by computing the largest wavelet scale across coordinates, applying one scale-dependent phase, and uncomputing.

## The trick

For a one-dimensional wavelet basis, the diagonal preconditioner used in the source paper acts as

$$
P|j\rangle=2^{-\lfloor\log_2 j\rfloor}|j\rangle,
$$

with trivial action on $|0\rangle$. The scale is the position of the most significant nonzero bit of $j$.

For a $d$-dimensional tensor-product wavelet basis, the preconditioner is not the product of $d$ independent one-dimensional scalings. It uses the largest scale index:

$$
P_{dD}|j_1\rangle\cdots |j_d\rangle
=2^{-\lfloor\log_2 j_{\max}\rfloor}|j_1\rangle\cdots |j_d\rangle,
\qquad
j_{\max}=\max(j_1,\ldots,j_d).
$$

To implement its unitary pair

$$
U^\pm_{dD}=e^{\pm i\arccos P_{dD}},
$$

use the reversible circuit pattern:

1. Compute $j_{\max}=\max(j_1,\ldots,j_d)$ into an $n$-qubit ancilla register.
2. Identify $\lfloor\log_2 j_{\max}\rfloor$ from the most significant nonzero bit.
3. Apply the phase $e^{\pm i\theta}$ with $\cos\theta=2^{-\lfloor\log_2 j_{\max}\rfloor}$.
4. Uncompute the maximum register.

This turns a multidimensional scale preconditioner into a modest amount of reversible comparison logic plus the same controlled rotations used in one dimension.

## When to reach for it

- Tensor-product wavelet discretisations or auxiliary wavelet bases for PDEs.
- Quantum algorithms where a multidimensional operator norm or scale depends on the largest coordinate frequency/scale.
- Circuit constructions where a diagonal operator depends on $\max(j_1,\ldots,j_d)$ rather than on all coordinates independently.

## Complexity

In the source paper, the reversible max operation costs

$$
O(dn)
$$

Toffoli gates and

$$
O(n)
$$

ancilla qubits for $d$ registers of $n$ qubits each. The controlled one-dimensional phase logic costs $O(n)$ additional gates. The dominant cost in the full PDE algorithm is still usually the $d$ quantum wavelet transforms, $O(dn^2)$ gates.

## Caveat

This card is circuit-level, not a theorem that wavelet preconditioning works. The condition-number guarantee comes from the PDE and wavelet-analysis assumptions in [[Wavelet Preconditioning for Quantum PDE Solvers]]. If those assumptions fail, implementing the diagonal preconditioner efficiently does not help.

## Related notes

- [[Fast Quantum Algorithm for Differential Equations (Bagherimehrab-Nakaji-Wiebe-Brennen-Sanders-Aspuru-Guzik 2023) — Paper Notes]]
- [[Wavelet Preconditioning for Quantum PDE Solvers]]
- [[Extended Observable Trick for Nonunitary Preconditioners]]
- [[Block-Encoding Composition Algebra]]
