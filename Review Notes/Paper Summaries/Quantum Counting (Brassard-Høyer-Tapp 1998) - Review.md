# Review: Quantum Counting (Brassard-Høyer-Tapp 1998)

Source note: [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]

## Primary Sources Checked

- Brassard, Høyer, and Tapp, "Quantum Counting," ICALP 1998, arXiv:quant-ph/9805082.

## Verdict

Minor-to-major issues. The note is a useful bridge between Grover search and later amplitude estimation, but a few theorem statements are too broad, especially around exact counting and "any heuristic" speedups.

## Line-Anchored Comments

- Lines 17-25: good high-level placement. This note correctly distinguishes the 1998 counting paper from the fuller 2002 amplitude-amplification treatment.
- Lines 31-45: the rotation formula is right, but the phase notation is potentially confusing. If `phi` and `varphi` are phase angles, the standard sign flip corresponds to phase `pi`, not the literal value `-1`; if the note means the operator eigenvalue, say "sign flip."
- Lines 47-51: exact amplification requires either phase adjustment or a carefully chosen initial rotation. Plain integer Grover iteration with `floor(pi/(4 theta))` gives high success, not exact success.
- Lines 57-61: the heuristic speedup is too sweeping. The result needs a reversible/checkable version of the randomized heuristic and a well-defined success distribution; it is not a free square-root speedup for arbitrary adaptive classical code.
- Lines 69-79: the frequency-extraction explanation is strong and should stay.
- Lines 84-90: the main approximate-counting error bound is accurately stated.
- Lines 96-103: the exact-counting cost needs endpoint caveats. A formula like `O(sqrt(tN))` cannot be right when `t=0`; exact counting/search has special handling for `t=0` and `t=N`, and later presentations use endpoint-safe variants.
- Lines 111-116: good summary table, but "exact counting `O(sqrt(tN))`" should be revised as above.
- Lines 133-136: controlled powers/depth caveat is important. Keep it near the algorithm, not only at the end.

## Missing or Suggested Additions

- Add a short "1998 vs 2002" note: this paper gives quantum counting and early amplification ideas; BHMOT 2002 standardizes the general amplitude-estimation theorem and exact-amplification variants.

