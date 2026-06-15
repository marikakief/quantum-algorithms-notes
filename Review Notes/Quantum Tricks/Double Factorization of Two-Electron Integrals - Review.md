# Review: Double Factorization of Two-Electron Integrals

Source note: [[Double Factorization of Two-Electron Integrals]]

## Primary Sources Checked

- Motta et al., arXiv:1808.02625 / npj Quantum Information 7, 83 (2021).

## Verdict

Minor issues. This is one of the cleaner trick cards in the tranche. It needs only sharper language around empirical scaling and what error is controlled.

## Line-Anchored Comments

- Line 7: the `O(N^2 log N)` improvement should be immediately labeled empirical/asymptotic for increasing molecule size. The card later says this at line 50, but the headline reads too theorem-like.
- Lines 11-15: the first-factorization description is good. The exact supermatrix indexing should match the paper's chemist-ordering convention; avoid changing indices casually in future edits.
- Lines 17-25: the second factorization and diagonal number-operator form are well stated.
- Lines 29-33: the circuit description correctly connects Givens rotations and fermionic swap networks. This is the right way to use the swap network in arbitrary-basis chemistry.
- Line 39: "near-term Trotter-based simulation where circuit depth ... is the bottleneck" is fine historically, but this decomposition is also used in fault-tolerant resource-estimation pipelines. Mention both if the card is meant to be reusable.
- Lines 44-46: the complexity lines are good, but "classical preprocessing `O(N^4)`" can be a bottleneck for large systems and should maybe appear in the caveat section as well.
- Line 52: excellent caveat. It correctly separates per-step compilation from Trotter error.

## Missing or Suggested Additions

- Add a one-sentence bridge to double-factorization qubitization, where the same integral factorization feeds block encodings rather than Trotter layers.

