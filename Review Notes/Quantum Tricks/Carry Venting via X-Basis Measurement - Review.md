# Review: Carry Venting via X-Basis Measurement

Source note: [[Carry Venting via X-Basis Measurement]]

## Primary Sources Checked

- Gidney, arXiv:2507.23079.

## Verdict

Minor issues. The card is strong and well scoped.

## Line-Anchored Comments

- Lines 12-24: the Z-redundancy and phasing-task story is clear.
- Line 16: the complement-carry identity is central. If the source has boundary conditions for carry-in/carry-out or modulo arithmetic, include them here.
- Lines 36-38: good complexity summary. Say whether the carry-xor resolution cost is for all internal carries or includes boundary carries.
- Lines 40-44: excellent caveat about trading qubit garbage for classical bookkeeping and deferred cost.

## Missing or Suggested Additions

- Add a one-sentence contrast with ordinary compute-copy-uncompute: venting is useful when the garbage is classically recoverable from surviving quantum data but expensive to coherently uncompute.

