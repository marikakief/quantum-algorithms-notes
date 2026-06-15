# Review: Any AND-OR Formula Can Be Evaluated in O(N^{1/2+o(1)}) Queries (Ambainis-Childs-Reichardt-Spalek-Zhang 2007)

Source note: [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]]

## Primary Sources Checked

- Childs, Reichardt, Spalek, Zhang, "Every NAND formula of size N can be evaluated in time N^{1/2+o(1)} on a quantum computer", arXiv:quant-ph/0703015: https://arxiv.org/abs/quant-ph/0703015
- Ambainis, "A nearly optimal discrete query quantum algorithm for evaluating NAND formulas", arXiv:0704.3628: https://arxiv.org/abs/0704.3628

## Verdict

Minor issues. This is a good synthesis of the two companion papers and the FOCS result. The algorithmic structure is accurately conveyed: weighted formula-tree Hamiltonian, Szegedy-style coined walk, phase estimation, and formula rebalancing. Most corrections are metadata/link precision and a few overcompressed spectral statements.

## Line-Anchored Comments

- Lines 1-4: title/source framing is acceptable but should clarify provenance.

  The FOCS paper combines the Childs-Reichardt-Spalek-Zhang paper and Ambainis's companion result. The source title in arXiv:quant-ph/0703015 is "Every NAND formula...", while the note title uses the FOCS-style AND-OR formulation.

- Line 19: "connects the coined walk to a continuous-time quantum walk via Szegedy"

  The paper uses a Szegedy/product-of-reflections spectral theorem to discretize a Hamiltonian-style spectral test. It is not the same continuous/discrete equivalence later developed by Childs 2010; avoid merging those two results.

- Lines 52-57: eigenphase map should be stated with convention care.

  The important fact is zero energy maps to phases around `pi/2` and `3pi/2` for the walk convention used. The displayed formula is plausible under the note's convention, but the signs are easy to get wrong; tie it to the theorem rather than treating it as a generic Szegedy formula.

- Lines 67 and 92: general-formula exponent is right.

  The source states `N^{1/2 + O(1/sqrt(log N))}`, hence `N^{1/2+o(1)}`.

- Line 107: "Childs-Cleve-Jordan-Yeung" is wrong.

  The author is David Yonge-Mallo, not Yeung. This is a small but visible citation error.

- Line 114: later context is good but should name the later result.

  Reichardt's span-program formula evaluation gives the cleaner later endpoint. Add the citation if this note is meant to be a historical spine.

- Line 143: "alternative route" sentence should say "balanced NAND trees".

  The CCJYM discrete-query NAND-tree note is for balanced binary NAND trees, not arbitrary formulas.

## Missing or Suggested Additions

- Add a one-line explanation of why input leaves evaluating to 1 are disconnected: this is the spectral encoding of NAND evaluation, not an arbitrary graph modification.
- Add explicit model labels: query complexity is counted in leaf-value oracle queries; classical preprocessing on the known formula tree is allowed.
