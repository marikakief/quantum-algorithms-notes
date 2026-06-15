# Review: Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020)

Source note: [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes]]

## Primary Sources Checked

- Lin and Tong, "Optimal polynomial based quantum eigenstate filtering with application to solving quantum linear systems," Quantum 4, 361 (2020), arXiv:1910.14596.

## Verdict

Major issues. The high-level importance of eigenstate filtering and its use in QLSP is right, but the note has serious bibliographic and technical errors: the authorship is wrong, the displayed Chebyshev filter is not the stated minimax filter, and the Hermitian-dilation condition-number claim is incorrect.

## Line-Anchored Comments

- Line 1 and the filename: the bibliographic attribution appears wrong. arXiv:1910.14596 and the Quantum publication list the authors as Lin Lin and Yu Tong. "Dong, Meng, Whaley, and Lin" is associated with later QSP phase-factor work, not this paper.
- Lines 7-11: the broad description is good once the authorship is fixed. The note should call this a Lin-Tong paper, not Dong-An-Lin.
- Lines 23-37: the displayed polynomial `T_d(lambda/delta)/T_d(1/delta)` cannot be the stated minimax filter on `D_delta = [-1,-delta] union [delta,1]`: at `lambda = 1` it equals 1, so the maximum error on `D_delta` is not `1/T_d(1/delta)`. The correct even Chebyshev construction maps `lambda^2` from `[delta^2,1]` to `[-1,1]` and evaluates the denominator at the mapped value of `lambda=0`, which lies outside `[-1,1]`.
- Lines 39-43: QSP implementation should be revisited after fixing the polynomial. The boundedness and parity conditions are essential, and the current formula does not support the claimed suppression on all of `D_delta`.
- Lines 45-49: the postselection/overlap discussion is mostly right for the general eigenstate-filtering primitive. Since line 9 says the QLSP algorithms avoid amplitude amplification, distinguish the general primitive from the structured QLSP path where overlaps are kept bounded.
- Lines 55-67: the AQC route is summarized well. Add that the path, schedule, and gap lower bound are highly structured for the embedded linear-system Hamiltonians; this is not a generic adiabatic-to-filtering compiler with the same complexity.
- Lines 69-80: the Zeno description is too informal in one place. Frequent projections do not remove the need for discretization and overlap control; they replace adiabatic tracking with a different controlled sequence of approximate projections.
- Lines 82-93: the comparison table is useful, but it should use the same query oracles and precision conventions across rows. Otherwise "queries to `U_H`" and "queries to `O_B`" can hide very different implementations.
- Line 100: standard Hermitian dilation doubles the dimension, not the condition number. The nonzero singular values of the dilation are the singular values of `A` (with signs as eigenvalues), so the condition number is essentially preserved unless additional padding/scaling conventions introduce separate effects.
- Line 101: phase-factor computation being "solved in subsequent work" should be softened. Later work greatly improves numerical phase-factor evaluation, but high degree can still be a practical compilation and robustness issue.

## Missing or Suggested Additions

- Rename or cross-reference the review/note title to Lin-Tong 2020 so future searches do not propagate the wrong authorship.
- Replace the polynomial section with the actual even minimax construction before this note is used as a source for the [[Chebyshev Polynomial Spectral Projector]] trick card.

