# Review: Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017)

Source note: [[Factoring Safe Semiprimes with a Single Quantum Query (Grosshans-Lawson-Morain-Smith 2017) — Paper Notes]]

## Primary Sources Checked

- Grosshans, Lawson, Morain, and Smith, "Factoring safe semiprimes with a single quantum query", arXiv:1511.04385 / Journal of Mathematical Cryptology 11 (2017).

## Verdict

Minor issues. The note captures the point of the paper well: one reinterprets a QOFA divisor using safe-semiprime structure. The review should preserve the idealized-QOFA caveat and avoid making the fixed-base trick sound like a general Shor improvement.

## Line-Anchored Comments

- Lines 11-21: good restriction to distinct safe primes and the special `5 | N` case.
- Lines 21-27: the QOFA output model is important. Keep the statement that the theorem is about the idealized `d = r/gcd(t,r)` output, not exact finite-QFT resource accounting.
- Lines 33-39: the contribution is characterized correctly as a classical wrapper improvement, not a new quantum subroutine.
- Lines 50-80: the `s = d` or `2d` post-processing is clear. In line 68, note that the set membership for `s` is conditional on the successful-output lemma.
- Lines 84-104: the base-2 order argument is good, but the proof sketch should explicitly use the assumptions `q_i > 2`, distinct safe primes, and `5 ∤ N`; otherwise small exceptional cases spoil the clean order set.
- Lines 145-151: the one-query success probability is correct under the paper's assumptions.
- Lines 153-162: good Shor comparison. Keep "at most `1/2`" separate from the simplified perfect-order calculation, because QOFA can return a proper divisor.
- Lines 170-174: the table is useful; label the metric as QOFA-call count so readers do not infer a smaller modular-exponentiation circuit.
- Lines 180-184: excellent caveats. The "never slower than SFA" caveat is exactly the right nuance.

## Missing or Suggested Additions

- Add a sentence explaining that fixed base `2` is valuable for reusable compiled circuits, but this is not the same as choosing a base using knowledge of the factors.
- When mentioning general composite "salvage tests", say that the evidence there is empirical and does not inherit the safe-semiprime theorem.

