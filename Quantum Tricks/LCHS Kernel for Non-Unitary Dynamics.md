
> **Source:** Dong An, Jin-Peng Liu, and Lin Lin, *Linear combination of [[Hamiltonian simulation]] for nonunitary dynamics with optimal state preparation cost*, arXiv:2303.01029, Phys. Rev. Lett. **131**, 150603 (2023); used in *Quantum algorithm for linear matrix equations*, arXiv:2508.02822  
> **Links:** [LCHS arXiv](https://arxiv.org/abs/2303.01029) · [matrix equations arXiv](https://arxiv.org/abs/2508.02822)  
> **Tags:** #trick #non-unitary #dissipative #hamiltonian-simulation

## Idea

Represent a decaying non-unitary evolution such as \(e^{-tQ}\), when the Hermitian part of \(Q\) is positive, as a linear combination / integral over unitary evolutions of related Hermitian generators.

Concretely: the LCHS identity writes the non-unitary propagator as a weighted integral (or sum) of unitary [[Hamiltonian simulation]] operators. This is fundamentally different from dilation-based approaches (which embed into a larger Hilbert space and then postselect) or from [[QSVT Meta-Template|QSVT]] (which uses the spectral mapping theorem). LCHS does not require either; it achieves optimal state preparation cost directly.

## Why it matters

Quantum circuits are unitary, but many useful operator transforms are not. The LCHS kernel is a principled way to bridge that gap: it turns dissipative evolution into something a Hamiltonian-simulation toolbox can implement.

In the matrix equations paper (2508.02822), this is used to handle the non-unitary semigroup factors that arise when the effective Sylvester operator has a positive Hermitian part.

## When to use it

Use this when a matrix-function algorithm naturally produces non-unitary semigroup evolution and the Hermitian part guarantees decay rather than growth. It is not applicable when the evolution is not a contraction (i.e., when the spectrum can grow).

## Caveat

Useful only when the resulting unitary family is implementable efficiently and the kernel discretization does not blow up the coefficient norm. In practice, the cost scales with the number of quadrature points needed to approximate the kernel integral.

## Related notes

- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]] — generalises to tuneable kernels with near-optimal precision scaling
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes]] — extends LCHS to general eigenvalue transforms via Laplace representation
- [[Generalised LCHS Identity]] — the generalised version of this identity with freely choosable kernel
- [[Laplace-Transform LCU Lifting for Eigenvalue Transforms]]
- [[Near-Optimal Hardy-Space Kernel for LCHS]]
- [[Phragmén–Lindelöf Decay Barrier for Analytic Kernels]]
- [[Quantum Linear Matrix Equations — Paper Notes]]
- [[Hubbard-Stratonovich Decoupling]]
- [[Linear Combination of Unitaries (LCU)]]
