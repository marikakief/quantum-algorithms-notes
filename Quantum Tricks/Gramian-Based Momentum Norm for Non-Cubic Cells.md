# Gramian-Based Momentum Norm for Non-Cubic Cells

> **Source:** Berry, Rubin, Babbush et al., arXiv:2312.07654
> **Tags:** #trick #plane-wave #non-cubic #arithmetic #block-encoding #first-quantized

## What it does

Computes the squared norm $\|k_\nu\|^2$ of a reciprocal-space vector for arbitrary (non-cubic, non-orthogonal) Bravais lattices using the reciprocal lattice Gramian matrix. Allows different numbers of qubits $(n_x, n_y, n_z)$ in each Miller index direction, maintaining uniform reciprocal-space resolution regardless of cell geometry.

## The trick

For a cubic cell, $k_\nu = 2\pi \nu / \Omega^{1/3}$, so $\|k_\nu\|^2 \propto \nu_x^2 + \nu_y^2 + \nu_z^2$ — a sum of three squares, computable with $O(n^2)$ Toffolis.

For general Bravais vectors $(a_1, a_2, a_3)$, the reciprocal vectors $g^{(i)}$ define a Gramian $g_{ij} = (g^{(i)})^T g^{(j)}$ (real, symmetric, positive definite), and:

$$\|k_\nu\|^2 = g_{11} \nu_x^2 + g_{22} \nu_y^2 + g_{33} \nu_z^2 + 2g_{12} \nu_x \nu_y + 2g_{23} \nu_y \nu_z + 2g_{13} \nu_x \nu_z$$

The worst-case arithmetic cost is $(5/2)(n_x^2 + n_y^2 + n_z^2) + 2(n_x + n_y + n_z)^2 + 4b(n_x + n_y + n_z)$ Toffolis, where $b$ is the precision for real-number arithmetic.

**The payoff comes from crystal symmetries.** Real lattices rarely have all six $g_{ij}$ independent. Structure-specific simplifications:

| Crystal class | Simplification | Arithmetic cost |
|---|---|---|
| Diamond (FCC) | Decomposes into 3 squares of $(\nu_x + \nu_y - \nu_z)$, etc. | $3n^2$ (no real-number multiplications!) |
| FCC metals (Pt, Pd, Rh) | $g_{11} = g_{22} = -2g_{12}$; $g_{23} = g_{13} = 0$ | $\max(n_x,n_y)^2 + n_z^2 + 2n_xn_y + 2n_z(n_z + b)$ |
| Monoclinic (LNO-C2/m) | $g_{11} = g_{22}$; $g_{23} = -g_{13}$ | Reduced to 2 squares + 2 products + 3 multiplications by constants |
| Orthorhombic (Li₀.₇₅MnO₂F) | $g_{22} = g_{33}$; all off-diagonal zero | $n_x^2 + n_y^2 + n_z^2 + 2n_x(n_x + b)$ |

The inner product $k_p \cdot k_q$ (needed for Legendre polynomials in the nonlocal pseudopotential) has exactly double the integer arithmetic cost, because squarings become products of different numbers. But the real-number multiplication cost is unchanged.

An important subtlety: since $\|k_\nu\|^2$ always gets multiplied by another constant elsewhere in the algorithm, one overall scale factor in the Gramian can be absorbed for free — eliminating one multiplication.

## When to reach for it

- First-quantized plane-wave simulations of any non-cubic material
- Any quantum algorithm requiring momentum-space norms or inner products in a non-orthogonal basis (e.g., lattice models with non-standard unit cells)
- The pattern generalises: whenever a quadratic form needs coherent evaluation, precompute the Gramian classically and exploit its symmetry structure

## Complexity

Worst case (triclinic, all $g_{ij}$ independent): $(5/2)(n_x^2 + n_y^2 + n_z^2) + 2(n_x + n_y + n_z)^2 + 4b(n_x + n_y + n_z)$.

Best case (FCC diamond): $3n^2$ — same as the cubic case.

Typical materials fall between these extremes. For $n \sim 6$, $b = 20$: worst case ~1,000 Toffolis, best case ~108 Toffolis.

## Caveat

The Gramian must be precomputed classically from the lattice geometry — it's baked into the circuit at compile time. Changing the lattice geometry requires recompilation. This is fine for materials simulation (you know the crystal structure before you run), but wouldn't work for geometry optimisation where the lattice vectors are dynamic.

For very low-symmetry crystals (triclinic), the overhead over the cubic case is substantial: a factor of ~5–10 in the norm computation. But this cost is typically small compared to the controlled swaps ($12\eta n$) that dominate the total block encoding.

## Related notes
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — The paper introducing this technique
- [[First-Quantized Plane-Wave Chemistry Encoding]] — The encoding framework this extends to non-cubic cells
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — Prior work restricted to cubic cells
