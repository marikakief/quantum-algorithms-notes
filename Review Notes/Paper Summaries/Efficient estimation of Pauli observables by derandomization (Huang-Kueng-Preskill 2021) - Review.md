# Review: Efficient estimation of Pauli observables by derandomization (Huang-Kueng-Preskill 2021)

Source note: [[Efficient estimation of Pauli observables by derandomization (Huang-Kueng-Preskill 2021) — Paper Notes]]

## Primary Sources Checked

- Huang, Kueng, and Preskill, "Efficient estimation of Pauli observables by derandomization", arXiv:2103.07510 / Physical Review Letters 127, 030503 (2021).

## Verdict

Minor issues. The note is accurate and useful. The main improvement is notation cleanup around the "hits" relation and the guarantee relative to optimal schedules.

## Line-Anchored Comments

- Lines 19-25: the measurement-setting model is clear. Define the hit relation symbol once; `o_l` obtained from `p_m` by replacing Paulis with identities is easy to get backwards.
- Lines 50-62: the confidence bound is correctly identified as the scalar objective.
- Lines 91-103: the conditional-expectation expression is dense. Add a prose sentence saying which factors account for previous hits, current partially assigned shot, and remaining random shots.
- Lines 160-168: good statement of the derandomization promise. Emphasize that it is no worse than the average random schedule, not globally optimal.
- Lines 192-198: the caveats are good and should stay.

## Missing or Suggested Additions

- Add a tiny example with two overlapping Pauli strings. This would make the conditional-expectation rule much easier to remember.

