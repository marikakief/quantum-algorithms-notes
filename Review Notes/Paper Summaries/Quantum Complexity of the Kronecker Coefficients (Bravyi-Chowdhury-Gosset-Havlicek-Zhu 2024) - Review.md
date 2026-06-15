# Review: Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024)

Source note: [[Quantum Complexity of the Kronecker Coefficients (Bravyi-Chowdhury-Gosset-Havlicek-Zhu 2024) — Paper Notes]]

## Primary Sources Checked

- Bravyi, Chowdhury, Gosset, Havlicek, and Zhu, "Quantum complexity of the Kronecker coefficients," arXiv:2302.11454; PRX Quantum 5, 010329 (2024).

## Verdict

Minor issues. The note correctly treats the paper as a complexity-placement result, not an efficient algorithm for exact or relative-error Kronecker coefficients in general.

## Line-Anchored Comments

- Lines 11-22: good problem setup. Unary encoding is critical and should stay near `ExactKron`/`ApproxKron`.
- Lines 30-34: the assessment is accurate. The projector-rank identity is the durable conceptual contribution.
- Lines 40-46: representation projectors are summarized well. This is a generalized irrep-label measurement, not just a character formula.
- Lines 50-60: the diagonal-invariance projector is clear. It would help to say explicitly that `Q` and the Fourier-label projectors commute because they are compatible group-action projectors.
- Lines 64-74: the verifier circuit is described at the right level. The Beals `S_n` QFT assumption should remain attached to the `poly(n)` circuit statement.
- Lines 78-86: strong explanation of the trace/rank identity.
- Lines 104-114: the additive normalized sampling caveat is important. This is the main place where a reader might incorrectly infer an efficient relative-error algorithm for `g`.
- Lines 118-125: result table is good. "ApproxKron reduces to QXC" is an upper-bound/reduction statement, not a standalone efficient classical or quantum algorithm.
- Lines 140-146: caveats are complete. Positivity in QMA is an upper bound, not a QMA-completeness result.

## Missing or Suggested Additions

- Add a short note connecting the additive sampler here to Larocca-Havlicek's exact-by-rounding algorithms in small dimension-ratio regimes.
- Add a "not a lower bound" warning near the QMA/#BQP claims; these are containments and reductions.

