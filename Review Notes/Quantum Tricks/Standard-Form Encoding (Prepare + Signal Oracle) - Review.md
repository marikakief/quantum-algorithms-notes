# Review: Standard-Form Encoding (Prepare + Signal Oracle)

Source note: [[Standard-Form Encoding (Prepare + Signal Oracle)]]

## Primary Sources Checked

- Low and Chuang, "Hamiltonian Simulation by Qubitization," arXiv:1610.06546: https://arxiv.org/abs/1610.06546

## Verdict

Major issues because the central normalization is missing from the displayed definition.

## Line-Anchored Comments

- Lines 8-12: the standard-form relation should show normalization: typically `H/alpha = (<G| tensor I) U (|G> tensor I)`. Writing `H = ...` silently assumes `alpha=1`.
- Line 16: "act on it uniformly" is true only after normalization and access-cost accounting. The normalization determines the query complexity of qubitization/QSP.
- Lines 22-24: the caveat correctly says normalization matters; that caveat should be promoted into the definition itself.

## Missing or Suggested Additions

- Add the tuple notation used in the source, e.g. standard-form `(H, alpha, U, d)`, or translate it to modern `(alpha, a, epsilon)` block-encoding language.
- Add one example: LCU PREPARE/SELECT gives `H/lambda` where `lambda = sum |w_l|`.
