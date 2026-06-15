# Review: Block-Diagonal Decomposition for Parallel Hamiltonian Simulation

Source note: [[Block-Diagonal Decomposition for Parallel Hamiltonian Simulation]]

## Primary Sources Checked

- Abrams and Lloyd, *Simulation of Many-Body Fermi Systems on a Universal Quantum Computer*, arXiv:quant-ph/9703054: https://arxiv.org/abs/quant-ph/9703054
- Lloyd, *Universal Quantum Simulators*, Science 273, 1073 (1996): https://doi.org/10.1126/science.273.5278.1073

## Verdict

Minor issues. The even/odd block decomposition is well captured. The card should distinguish identical-block translational invariance from the more general "disjoint commuting blocks" case.

## Line-Anchored Comments

- Lines 10-16: Correct for a uniform one-dimensional nearest-neighbor hopping Hamiltonian. The even/odd split creates disjoint 2x2 blocks.

- Line 16: The relabeling formula is indexing-convention dependent. Specify whether sites are zero- or one-indexed to avoid off-by-one confusion.

- Lines 18-23: Good generalization to graph coloring, but be precise: bounded-degree graphs need a constant number of edge colors only when degree is bounded independently of system size.

- Line 25: The `O((ln m)^2)` operation count is tied to the relabeling/arithmetic model in the source. In a circuit model with explicit qubit layout, routing and basis-change costs may be different.

- Line 28: Good caveat. Nonuniform couplings keep block structure but require block-dependent phases/rotations.

## Missing or Suggested Additions

- Add "identical blocks" versus "disjoint nonidentical blocks" as separate cases.
- Mention that this is a product-formula grouping trick: terms within a color commute, terms between colors generally do not.
