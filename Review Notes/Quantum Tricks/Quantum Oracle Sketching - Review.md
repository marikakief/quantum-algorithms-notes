# Review: Quantum Oracle Sketching

Source note: [[Quantum Oracle Sketching]]

## Primary Sources Checked

- Zhao et al., arXiv:2604.07639, checked current arXiv v1.

## Verdict

Minor issues. The card captures the idea well, but its sample-complexity and phase-oracle formulas are too schematic.

## Line-Anchored Comments

- Lines 12-18: verify the exact distribution weighting in the phase oracle. The paper's sample model is distribution-dependent; a displayed `p(x) f(x)` factor should not be used unless it is exactly the paper's convention.
- Lines 19-21: the compositional story is good.
- Lines 32-38: `M ~ N Q^2` is a useful mnemonic, but the theorem has model-dependent factors: precision, repetition/correlation number, distribution parameters, and downstream query details. Add a pointer to the exact theorem before using the formula elsewhere.
- Lines 40-44: good caveat. Keep the warning that oracle sketching does not make data loading free.

## Missing or Suggested Additions

- Add a "not qRAM" sentence in the first paragraph; this card is likely to be cited exactly when someone is tempted to hide input access.

