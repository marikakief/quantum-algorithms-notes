# Review: Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009)

Source note: [[Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) — Paper Notes]]

## Primary Sources Checked

- Childs, Cleve, Jordan, Yonge-Mallo, "Discrete-query quantum algorithm for NAND trees", arXiv:quant-ph/0702160, Theory of Computing 2009: https://arxiv.org/abs/quant-ph/0702160
- Farhi, Goldstone, Gutmann, Hamiltonian NAND-tree algorithm, cited by the source.

## Verdict

Minor issues. The note correctly identifies the paper as a short conversion from the FGG Hamiltonian-query NAND-tree algorithm to the standard query model. The main correction is the FGG runtime: the source abstract states `O(sqrt(N log N))`, not `O(sqrt(N) log N)`.

## Line-Anchored Comments

- Lines 19, 33, 70: FGG runtime is misstated.

  The source abstract says Farhi-Goldstone-Gutmann run in time `O(sqrt(N log N))` in the Hamiltonian query model. The note writes `O(sqrt(N) log N)`, which is a larger logarithmic overhead.

- Line 21: "pins the quantum query complexity ... to Theta(N^{1/2+o(1)})"

  Fine as a statement of this paper plus Barnum-Saks, but the note should also say that this is not the final word for balanced NAND trees: the companion FOCS line obtains the optimal `O(sqrt(N))` query result for balanced/approximately balanced formulas.

- Lines 37-43: exact two-query simulation of `e^{-iH_O t}` is the core point and is well explained.

  Good. Keep the oracle convention explicit: this works for the specific self-inverse standard query encoding used in the paper.

- Lines 47-53: product-formula overhead is right at the level needed here.

  Add that the Suzuki order is a fixed constant chosen as a function of the desired `epsilon`; it is not varied with `N` inside the asymptotic statement.

- Line 76: "ACRSZ" naming is fine, but the note should say the approaches appeared contemporaneously.

  This paper is a bridge for FGG; ACRSZ/Ambainis is the broader formula-evaluation solution.

- Line 86: "does not apply to time-dependent driving Hamiltonians"

  Good caveat, but phrase as "not by this simple argument"; later gate-efficient/fractional-query simulations handle more general continuous-time query algorithms.

## Missing or Suggested Additions

- Add the arXiv chronology: preprint in 2007, Theory of Computing publication in 2009, and arXiv v2 in 2019 only updates an author's name.
- Mention that the paper is two pages because the substantial algorithmic work is in FGG and in sparse-Hamiltonian simulation.
