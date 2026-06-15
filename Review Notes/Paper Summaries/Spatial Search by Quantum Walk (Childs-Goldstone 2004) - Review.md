# Review: Spatial Search by Quantum Walk (Childs-Goldstone 2004)

Source note: [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]]

## Primary Sources Checked

- Childs and Goldstone, "Spatial search by quantum walk", arXiv:quant-ph/0306054, Phys. Rev. A 2004: https://arxiv.org/abs/quant-ph/0306054
- Aaronson and Ambainis, "Quantum search of spatial regions", Theory of Computing 2005, cited in the source abstract/context.

## Verdict

Major issues in the dimension table and sign conventions. The main headline is correct: the continuous-time walk gives full `sqrt(N)` search on periodic lattices only above dimension 4, logarithmic degradation at dimension 4, and no substantial speedup below. But the note mixes raw evolution time with total cost after accounting for success probability, and the Laplacian convention is easy to misread.

## Line-Anchored Comments

- Lines 21-25: Hamiltonian/Laplacian convention needs cleanup.

  The source uses a convention equivalent up to an overall sign to `gamma L - |w><w|`, with the standard graph Laplacian normally `D-A`. The note writes `H = -gamma L + |w><w|` and then defines `L = A-D`. That can be made consistent, but only because both signs have been flipped. Spell this out, because the "ground state" language later depends on the convention.

- Lines 33-40: "reduces to a 2D subspace"

  This is a useful teaching simplification, but not literally an exact two-dimensional invariant subspace on lattices. The proof controls the low-energy behavior through lattice sums and the resolvent equation. Say "effectively reduces the search dynamics to the two relevant low-energy states" rather than "reduces to a 2D subspace".

- Lines 56-63: table mixes evolution time and search complexity.

  For `d=4`, the listed time `O(sqrt(N log N))` comes with success probability `O(1/log N)`, so the total cost for constant success has additional amplification/repetition overhead. For `d=3`, the table's "O(N^{2/3}) total" is especially misleading: the success probability vanishes as a power of `N`, so raw evolution time is not a successful search complexity.

- Line 69: "upper critical dimension"

  This is good intuition and consistent with the lattice-sum threshold, but keep it anchored to the algorithmic analysis. Avoid implying that Childs-Goldstone prove a statistical-mechanical phase transition theorem.

- Lines 86-90: comparison to Aaronson-Ambainis and AKR is useful.

  Add model labels: Aaronson-Ambainis is a spatial-region algorithm, AKR is a coined discrete-time walk, and Childs-Goldstone is a single continuous-time Hamiltonian walk. The models have different implementation assumptions.

- Line 119: link target for Boyer-Brassard-Hoyer-Tapp is wrong.

  The text cites BBHT tight bounds, but the link points to the BHMT amplitude-amplification/estimation paper. Use the BBHT review/source note instead.

## Missing or Suggested Additions

- Separate "Hamiltonian evolution time" from "cost to obtain a marked vertex with constant probability".
- Add a short note that the complete graph and hypercube cases are not finite-dimensional lattices; they are included to connect to analog Grover and high-connectivity search.
