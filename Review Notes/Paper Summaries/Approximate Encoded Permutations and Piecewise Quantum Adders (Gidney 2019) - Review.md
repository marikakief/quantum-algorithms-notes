# Review: Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019)

Source note: [[Approximate Encoded Permutations and Piecewise Quantum Adders (Gidney 2019) — Paper Notes]]

## Primary Sources Checked

- Gidney, "Approximate encoded permutations and piecewise quantum adders", arXiv:1905.08488 (2019).

## Verdict

Minor issues. The note is strong. It just needs slightly sharper language around the asymptotic depth claim and the error accounting.

## Line-Anchored Comments

- Lines 13-18: the approximate encoded permutation framework and the two constructions are well summarized.
- Line 19: the `O(log log n)` depth statement is correct only after adding several conditions: `O(log n)` piece size, `O(log n)` padding, polynomially many additions/error budget, and carry-lookahead on each piece. Keep all of those conditions in the same sentence.
- Lines 45-55: the coset representation section is clear.
- Lines 57-69: the oblivious runway construction is well explained. Make explicit that the runway value is part of an encoded representation, not a classical carry register.
- Lines 73-78: good error accounting. Say whether `k` is the number of additions in one encoded computation and whether the bound is a union-bound style composition.
- Lines 97-99: the practical Shor estimate is useful, but it is tied to surface-code/factory assumptions. Name those assumptions before saying "lowest spacetime volume."
- Lines 114-124: the caveats are good and should remain.

## Missing or Suggested Additions

- Add a contrast with "optimistic circuits": both allow rare bad cases, but encoded permutations bound error through a random coset/padding choice while optimistic circuits use Frobenius-average circuit error.

