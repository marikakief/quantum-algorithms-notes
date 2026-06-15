# Review: Computational Complexity of Interacting Electrons and Fundamental Limitations of DFT (Schuch-Verstraete 2009)

Source note: [[Computational Complexity of Interacting Electrons and Fundamental Limitations of DFT (Schuch-Verstraete 2009) — Paper Notes]]

## Primary Sources Checked

- Schuch and Verstraete, "Computational complexity of interacting electrons and fundamental limitations of density functional theory," Nature Physics 5, 732-735 (2009) / arXiv:0712.0483.

## Verdict

Major issue in one formula; otherwise strong. The note gives the right complexity-theoretic story, but the Hubbard-to-Heisenberg coupling is likely off by a convention-sensitive factor, and the DFT oracle conclusion should be phrased less strongly.

## Line-Anchored Comments

- Lines 7-15: the input/output statement is clear. Keep the local magnetic fields prominent; without them the reader may incorrectly infer hardness for the plain translationally invariant Hubbard model.
- Lines 19-23: good summary, but "exactly as hard to compute as the ground state energy itself" should be marked as an oracle/reduction statement. It is not saying every practical approximate functional is worst-case QMA-hard at chemical accuracy.
- Lines 31-37: the reduction chain is helpful. Add a note that the gadgets introduce polynomially large couplings and precision bookkeeping; otherwise the chain looks more physically natural than it is.
- Lines 39-49: the mediator-gadget sketch is useful, but this is a place where constants and signs are fragile. It is fine for a review note, but should not be treated as a derivation.
- Lines 51-54: likely correction needed. For the usual half-filled Hubbard model with spin operators `S_i`, the antiferromagnetic Heisenberg exchange is `J = 4t^2/U` up to convention-dependent constants. The note's `J = 2t^2/U` should be checked against the paper's convention before publishing.
- Lines 55-65: the DFT implication is basically right. Replace `P^F = QMA` with a more careful oracle-hardness statement: an efficient sufficiently accurate oracle for the universal functional would allow solving the QMA-hard Hubbard instances by polynomial-time optimization.
- Lines 69-75: key results are good. The Hartree-Fock NP-completeness appendix result is worth keeping, but should be identified as a separate classical variational problem rather than a direct corollary of the DFT theorem.
- Lines 92-99: the caveats are excellent and should remain. They are essential for preventing overinterpretation of the DFT claim.

## Missing or Suggested Additions

- Add a "precision regime" sentence: the hardness is for inverse-polynomial energy precision in worst-case constructed instances, not for the empirical success or failure of approximate DFT on ordinary molecules.
- Add a convention note for Hubbard-to-Heisenberg exchange, since different normalizations of Pauli matrices versus spin operators change factors of two or four.

