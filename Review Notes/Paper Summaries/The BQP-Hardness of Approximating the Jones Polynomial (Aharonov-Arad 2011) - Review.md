# Review: The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011)

Source note: [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes]]

## Verdict

Minor issues. The note is concise and mostly accurate. The main improvements are metadata consistency, exact parameter conventions, and care around the multiplicative-hardness implication.

## Line-Anchored Comments

- Line 1: The vault filename says 2011, while the source line gives Israel Journal of Mathematics 176 (2010). Add a metadata note explaining arXiv/original/publication dates so this does not look like a contradiction.

- Lines 7-17: The problem statement should include the normalization/additive scale from AJL. "To within the AJL accuracy" is fine for insiders, but the note should say what object is being approximated and how the promise is normalized.

- Lines 29-37: The Bridge and Decoupling Lemmas are described at the right level. Add whether they prove irreducibility, density, or Lie-algebra generation in each use; those are related but not identical.

- Lines 39-51: The encoding paragraph should specify the path-model subspace and leakage control. "Two paths of length 4" is too compressed for a reader trying to reconstruct the reduction.

- Lines 55-61: The `k >= 5`, `k != 6` range should be tied to the paper's root-of-unity convention. Jones-polynomial papers use several incompatible `k/r/q` notations.

- Line 61: The Kuperberg multiplicative-hardness implication needs more precision. State the exact approximation regime and closure type before saying "#P-hard."

## Missing or Suggested Additions

- Add a small table aligning AJL, Freedman-Larsen-Wang, and Aharonov-Arad parameter conventions.
- Add a one-line explanation of why trace closure and plat closure have different complexity status.
