# Review: A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017)

Source note: [[A Quantum Primality Test with Order Finding (Donis-Vela-Garcia-Escartin 2017) — Paper Notes]]

## Primary Sources Checked

- Donis-Vela and Garcia-Escartin, "A quantum primality test with order finding", arXiv:1711.02616 (2017).

## Verdict

Minor issues. The note correctly identifies the paper as a certificate-model tradeoff rather than a new quantum primitive. The main thing to sharpen is the distinction between prime-input success probability, composite-input rejection probability, and classical verifiability of the certificate.

## Line-Anchored Comments

- Lines 15-19: good output/cost framing. The phrase "quantum-checkable certificate" is exactly right and should be kept prominent.
- Lines 21-27: the Lucas converse is stated cleanly. This is the right proof idea: order divides `phi(N)`, and `phi(N) < N-1` for composite `N > 1`.
- Lines 31-33: good assessment. The note should keep saying "saves factoring `N-1`" rather than implying a stronger primality theorem.
- Lines 65-78: the loop is accurate, but line 74 should be phrased carefully. If `a^((N-1)/2)` is not `+-1`, then indeed `a^(N-1) != 1`; this is a Fermat witness, not merely an Euler-style witness.
- Lines 104-114: the Miller-Rabin preselection discussion is useful. Mention that the `4^{-k}` survival bound is for composite inputs under independent random bases, not for finding primitive roots on prime inputs.
- Lines 148-156: excellent certificate caveat. This should be repeated in the comparison table because a base `a` alone is not a Pratt/Lucas certificate unless `N-1` is factorized or order-finding is available.
- Lines 174-200: the two extra logarithmic factors are easy to confuse. Separate the expected number of candidate bases from the number of period samples needed to reconstruct an exact order.
- Lines 204-210: the table is useful. Add a footnote that the quoted Chau-Lo and Donis-Vela bounds use the paper's arithmetic model, not modern fault-tolerant modular-exponentiation resource estimates.
- Lines 214-218: the caveats are strong and should remain near the top if the note is shortened.

## Missing or Suggested Additions

- Add one sentence saying the algorithm is one-sided for prime certification: a found order `N-1` proves primality, but failure to find one after the chosen number of trials is only a probabilistic/computational stopping condition.
- In the prime-power side remark, distinguish "finds a primitive root of the unit group" from "detects arbitrary prime powers"; primitive roots exist only for the listed moduli.

