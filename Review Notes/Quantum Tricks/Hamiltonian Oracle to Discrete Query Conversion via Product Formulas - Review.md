# Review: Hamiltonian Oracle to Discrete Query Conversion via Product Formulas

Source note: [[Hamiltonian Oracle to Discrete Query Conversion via Product Formulas]]

## Primary Sources Checked

- Childs, Cleve, Jordan, Yonge-Mallo, arXiv:quant-ph/0702160: https://arxiv.org/abs/quant-ph/0702160
- Berry, Cleve, Gharibian, arXiv:1211.4637, for later gate-efficient conversion context: https://arxiv.org/abs/1211.4637

## Verdict

Major scope issue. The card states a useful trick, but line 7 says "any quantum algorithm" too broadly. The CCJYM observation applies to a specific Hamiltonian-oracle model with a self-inverse standard oracle and a time-independent driving Hamiltonian, and the simple product-formula conversion does not address gate efficiency.

## Line-Anchored Comments

- Line 7: "Converts any quantum algorithm..."

  Too broad. Say "converts Hamiltonian-query algorithms of the CCJYM/FGG form" or "continuous-time oracle algorithms with this oracle Hamiltonian and suitable driving Hamiltonian". Arbitrary continuous-time algorithms need additional assumptions.

- Lines 12-17: exact simulation of `e^{-iH_O t}` is correct for the model.

  Attach the model: the standard query oracle is self-inverse and encodes bits so that the controlled rotation plus query-unquery implements the fractional oracle exactly.

- Lines 19-21: product-formula overhead statement is right at high level.

  Add that `p` is fixed after choosing the desired exponent slack. Otherwise readers may think the order is tuned dynamically without cost.

- Lines 24-26: "need to express it in the standard query model"

  Good use case, but include "for query-complexity upper bounds". The card itself does not give a practical gate-efficient circuit.

- Lines 31-34: caveat is strong and should move into the "What it does" section.

  The lack of gate-efficiency is not a footnote; it is exactly why Berry-Cleve-Gharibian is a separate later paper.

## Missing or Suggested Additions

- Add a small "does not cover" list: arbitrary non-self-inverse Hamiltonian oracles, general time-dependent driving without further simulation machinery, and elementary-gate complexity.
