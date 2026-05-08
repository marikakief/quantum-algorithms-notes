# Wavelet Preconditioning for Quantum PDE Solvers

> **Source:** Bagherimehrab, Nakaji, Wiebe, Brennen, Sanders & Aspuru-Guzik, arXiv:2306.11802
> **Tags:** #trick #PDE #QLSA #preconditioning #wavelets #linear-systems

## What it does

Turns a badly conditioned finite-difference PDE linear system into a constant-condition-number system by changing to a wavelet basis and applying a diagonal scale preconditioner.

## The trick

Start with a discretised PDE linear system

$$
Au=b.
$$

Instead of applying a [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]] directly to $A$, move into a wavelet basis:

$$
u_W=Wu,\qquad b_W=Wb,\qquad A_W=WAW^T.
$$

Then apply a diagonal preconditioner $P$ in that basis:

$$
A_P=P A_W P,\qquad b_P=P b_W,\qquad u_P=P^{-1}u_W.
$$

For elliptic PDEs whose induced bilinear form is symmetric, bounded, and coercive, classical wavelet theory gives a preconditioner with

$$
\kappa(A_P)=O(1)
$$

independent of the grid size $N$. A [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] / QLSA inverse block-encoding then pays only for the constant condition number of $A_P$, not the polynomially growing condition number of the original finite-difference matrix.

The useful point: the PDE can still be discretised by finite differences. Wavelets serve as an auxiliary basis for linear algebra, not necessarily as the representation in which the problem is supplied.

## When to reach for it

- PDE algorithms where the discretised matrix has $\kappa(A)=\mathrm{poly}(N)$ but the operator is elliptic/coercive.
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]] applications where a classical numerical-analysis preconditioner has a fast quantum implementation.
- Quantum simulation or differential-equation solvers where basis choice can expose scale structure.

## Complexity

For the Bagherimehrab et al. construction, preparing the extended solution state uses

$$
O(\log(1/\varepsilon))
$$

queries to the block-encoding of $A$ and

$$
O(dn^2)
$$

gates for $N=2^{nd}$ grid points. The $O(n^2)$ term comes from the quantum wavelet transform. If the problem is supplied directly in the wavelet basis, the paper argues this can drop to $O(n)$ gate overhead, but then the input oracles must also be wavelet-basis oracles.

## Caveat

This is not a generic way around the QLSA lower bound. It works when the PDE promise supplies a good wavelet preconditioner. Hyperbolic, nonlinear, strongly non-normal, or badly behaved variable-coefficient problems need separate analysis.

It also only prepares a quantum state for feature extraction. Full classical readout still costs $\Omega(N)$.

## Related notes

- [[Fast Quantum Algorithm for Differential Equations (Bagherimehrab-Nakaji-Wiebe-Brennen-Sanders-Aspuru-Guzik 2023) — Paper Notes]]
- [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes]]
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes]]
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]]
- [[Preconditioning Quantum Linear Systems via Fast Inversion]]
- [[Block-Encoding Composition Algebra]]
