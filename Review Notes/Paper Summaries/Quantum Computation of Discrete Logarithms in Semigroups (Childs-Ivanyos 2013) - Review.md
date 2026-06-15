# Review: Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013)

Source note: [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes]]

## Primary Sources Checked

- Childs and Ivanyos, "Quantum computation of discrete logarithms in semigroups", arXiv:1310.6238 (2013).

## Verdict

Minor issues. This is a strong summary of a short but subtle paper. The main editorial improvement is to keep the three problems separate: ordinary one-generator semigroup DLP, shifted semigroup DLP, and multi-generator constructive membership.

## Line-Anchored Comments

- Lines 11-33: good problem taxonomy. The input bound `N` on `t+r` matters and should not be treated as a harmless constant.
- Lines 35-43: excellent high-level assessment. The "lack of inverses is harmless" line is true only for ordinary one-generator DLP.
- Lines 47-78: the period-finding construction is accurate. The tail-skipping probability depends on choosing `M > N^2+N`, which is worth keeping in the text.
- Lines 80-88: the binary search for `t` is clean.
- Lines 92-116: the cycle-case DLP reduction is good. Since `x` lies in the cycle, it is inside the extracted group; otherwise powers of `x` would be suspect in a general semigroup.
- Lines 118-137: good shifted-DLP-to-dihedral-HSP reduction. State explicitly that this is an upper-bound reduction to DHSP-style algorithms, not a hardness equivalence.
- Lines 139-193: the constructive-membership lower bound is well explained. The permutation-inversion reduction is the right black-box obstruction.
- Lines 201-223: the theorem list is useful. Keep query complexity and time complexity separate for shifted DLP and constructive membership.
- Lines 242-246: excellent caveats; preserve them.

## Missing or Suggested Additions

- Add a small "do not conflate" paragraph: ordinary semigroup DLP is polynomial-time quantum; shifted semigroup DLP is subexponential via known DHSP solvers; constructive membership has a black-box square-root-type lower bound.
- Mention that explicit semigroups can have extra structure outside the black-box model, so the constructive-membership lower bound is not universal cryptographic evidence.

