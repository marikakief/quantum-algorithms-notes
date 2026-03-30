# Jordan-Wigner Reduced Density Matrix Construction

> **Source:** Osborne and Nielsen, arXiv:quant-ph/0202162; Barouch and McCoy, PRA 2:1075 (1970)
> **Tags:** #trick #Jordan-Wigner #reduced-density-matrix #correlation-functions #exactly-solvable

## What it does
Constructs exact reduced density matrices for one or two sites of a spin-$\frac{1}{2}$ lattice model by: (1) solving the model via the Jordan-Wigner transform, (2) computing spin-spin correlation functions in the fermion basis, and (3) reconstructing the density matrix via the Pauli operator expansion.

## The trick
For a translationally invariant spin-$\frac{1}{2}$ chain, the two-site reduced density matrix $\rho_{0r}$ of sites separated by distance $r$ can be written:

$$\rho_{0r} = \frac{1}{4}\left(I + \langle\sigma^z\rangle(\sigma_0^z + \sigma_r^z) + \sum_{k=1}^{3}\langle\sigma_0^k\sigma_r^k\rangle\,\sigma_0^k\sigma_r^k\right)$$

where the symmetries of the Hamiltonian (reality, global phase flip $\prod_j \sigma_j^z$, translational invariance, reflection symmetry) kill all off-diagonal Pauli terms except the three diagonal correlators $\langle\sigma^x\sigma^x\rangle$, $\langle\sigma^y\sigma^y\rangle$, $\langle\sigma^z\sigma^z\rangle$ and the transverse magnetisation $\langle\sigma^z\rangle$.

The correlators are obtained in the Jordan-Wigner fermion basis, where $\langle\sigma_0^x\sigma_r^x\rangle$ and $\langle\sigma_0^y\sigma_r^y\rangle$ are Toeplitz determinants of the function $G_r$ (a one-dimensional integral in the thermodynamic limit), and $\langle\sigma_0^z\sigma_r^z\rangle = 4\langle\sigma^z\rangle^2 - G_r G_{-r}$.

**Key step:** The symmetry analysis reduces the 16-parameter two-site density matrix to 5 independent quantities. This makes it tractable to compute entanglement measures (concurrence, entanglement of formation) analytically.

## When to reach for it
- Computing entanglement in exactly solvable spin chains (XY model, transverse Ising, XX model, etc.)
- Any situation where you need reduced density matrices of a free-fermion model — the Jordan-Wigner + Wick's theorem combination gives all correlation functions
- Benchmarking numerical methods: the analytic density matrices provide exact references for testing DMRG, MPS, or other tensor network methods

## Complexity
In the thermodynamic limit: each correlation function is a one-dimensional integral (or Toeplitz determinant of size $r$). For fixed $r$, this is $O(r^3)$ via standard determinant evaluation. For the single-site density matrix: a single integral.

## Caveat
Only works for free-fermion models (quadratic Hamiltonians after Jordan-Wigner). Interacting models (e.g., XXZ with $\Delta \neq 0$ anisotropy in $z$, or models with next-nearest-neighbour interactions that don't map to free fermions) require different methods (Bethe ansatz, corner transfer matrix, etc.).

Also, the Jordan-Wigner transform in 1D is clean; in higher dimensions, string operators become non-local and more difficult to handle.

## Related notes
- [[Entanglement in a Simple Quantum Phase Transition (Osborne-Nielsen 2002) — Paper Notes]]
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]
- [[A Unified Graph-Theoretic Framework for Free-Fermion Solvability (Chapman-Elman-Mann 2023) — Paper Notes]]
