# Review: Triply Efficient Shadow Tomography (King-Gosset-Kothari-Babbush 2024)

Source note: [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]]

## Primary Sources Checked

- PRX Quantum metadata: https://journals.aps.org/prxquantum/abstract/10.1103/PRXQuantum.6.010336

## Verdict

Minor issues. This is an excellent summary, and the note correctly highlights the two-copy separation and the graph-coloring machinery. The main fixes are to distinguish sample-efficient existence results from triply efficient constructive algorithms, and to mark the harsh epsilon dependence earlier.

## Line-Anchored Comments

- Lines 25-33: The phrase "two-copy measurements are both necessary and sufficient" should specify the observable families. For `k`-local Paulis, one-copy classical shadows are already triply efficient; the two-copy necessity is for the full Pauli and fermionic regimes discussed later.

- Lines 37-43: Bell sampling estimates squared expectations. The note says magnitudes using `O(log |S| / epsilon^4)` samples, which is right at the headline level; add the reason for `epsilon^-4`: estimating squares accurately enough to threshold magnitudes.

- Lines 49-55: The sign-learning reduction is well explained. Be explicit that the graph is built on the large-magnitude set after stage 1, not on all of `S`.

- Lines 79-83: Theorem 6 for arbitrary `S` should be separated from Theorems 7 and 10. It gives a two-copy sample-efficient protocol, but not necessarily the same computational efficiency for every subset unless the coloring/mimicking step is efficient.

- Lines 83-88: The `k=2` and `k=3` epsilon exponents are valuable. Move a shorter version into the headline summary; otherwise "triply efficient" may sound practically efficient for all constant `k`.

- Lines 90-104: Corollary 12 and rapid retrieval should keep the constant-error assumption adjacent to the claim. The note does mention this in caveats; put it in the result line too.

## Missing or Suggested Additions

- Add a one-line definition of the commutation graph convention: edges are anticommutation conflicts or commuting-measurability conflicts? This prevents confusion because both graph choices appear in the literature.
- Add a small "sample efficient vs computationally efficient vs few-copy" table for Theorems 6, 7, and 10.
