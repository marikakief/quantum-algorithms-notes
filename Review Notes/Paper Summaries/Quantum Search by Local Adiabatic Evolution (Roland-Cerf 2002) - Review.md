# Review: Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002)

Source note: [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]

## Primary Sources Checked

- Roland and Cerf, "Quantum Search by Local Adiabatic Evolution," Physical Review A 65, 042308 (2002) / quant-ph/0107015.

## Verdict

Minor issues. The note is clear and appropriately concise. Only small wording fixes are needed.

## Line-Anchored Comments

- Lines 13-17: the summary is good. "No choice of `s(t)` can beat `O(sqrt(N))`" should be scoped to this adiabatic search/oracle Hamiltonian setting.
- Lines 21-35: the setup and spectrum are correct. At line 53, say "minimum gap" rather than "where the gap closes"; the gap is small but nonzero for finite `N`.
- Lines 39-53: the local condition and integrated schedule are well presented. Add that the matrix element in the adiabatic condition is also part of the precise derivation, even though it simplifies nicely here.
- Lines 57-65: good optimality sketch. It is worth saying this is an oracle lower bound, aligning it with Grover optimality.
- Lines 80-88: the broader significance is fair. The note should not imply local scheduling is generally available; the limitations section already states the gap-knowledge issue.

## Missing or Suggested Additions

- Add one sentence connecting the schedule to the continuous-time query model: the gain comes from spending almost all time in the narrow avoided-crossing region, matching Grover's lower bound.

