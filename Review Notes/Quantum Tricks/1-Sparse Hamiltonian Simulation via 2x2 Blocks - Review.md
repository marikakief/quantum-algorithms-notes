# Review: 1-Sparse Hamiltonian Simulation via 2x2 Blocks

Source note: [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]

## Primary Sources Checked

- Berry, Ahokas, Cleve, Sanders, arXiv:quant-ph/0508139: https://arxiv.org/abs/quant-ph/0508139

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 11-20: the block decomposition and eigenvalue formula are right.
- Line 22: "one oracle query" depends on the exact oracle interface. Some models need separate access to the neighbor and to diagonal/off-diagonal matrix values; safer wording is `O(1)` black-box calls for the relevant block.
- Lines 30-32: `O(1)` gates per block ignores finite-precision arithmetic for computing the rotation angle. This is fine in a query-complexity card, but should be labeled as such.
- Line 32: the phrase "you only touch the block containing your input state" is intuitive but slightly misleading for a coherent superposition over many basis states. The circuit computes the relevant block data coherently for each basis component.

## Missing or Suggested Additions

- Add a caveat that Hermiticity ties the two directed edges together; the oracle convention must avoid assigning inconsistent values to `H_xy` and `H_yx`.
