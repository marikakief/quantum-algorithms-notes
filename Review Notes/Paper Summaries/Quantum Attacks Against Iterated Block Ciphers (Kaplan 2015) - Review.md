# Review: Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015)

Source note: [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes]]

## Primary Sources Checked

- Kaplan, "Quantum attacks against iterated block ciphers," arXiv:1410.1434v2.

## Verdict

Minor issues. The note is accurate and has the right emphasis: the double-encryption lower bound is the durable theorem, while the four-encryption attack is an upper bound without a matching lower bound.

## Line-Anchored Comments

- Lines 11-18: good model statement. Quantum inverse queries are part of the oracle model and should remain explicit.
- Lines 21-36: the claw-finding reduction is clear. The uniqueness promise is important for the clean key-extraction formulation.
- Lines 40-48: good explanation of why four plaintext/ciphertext pairs are used. This prevents the reader from assuming the problem is defined with only one pair as in the two-key case.
- Lines 52-55: accurate assessment. Quantum double encryption gets real security amplification relative to single-key Grover search, unlike the classical MITM intuition.
- Lines 72-87: the time-space-product warning is valuable. The fastest attack is not necessarily the best low-memory attack.
- Lines 91-119: strong lower-bound explanation. The key point is that arbitrary ideal-cipher queries do not beat the projected claw-finding adversary matrix.
- Lines 145-164: the four-encryption quantum walk is well summarized. State the `M asymp N` assumption whenever quoting `N^{7/6}`.
- Lines 231-238: caveats are complete. Keep the no-general-`r` result warning.

## Missing or Suggested Additions

- Add a one-line "comparison target" note: all costs are ideal-cipher query/time/memory costs, not AES circuit resources.
- For teaching use, include a small table contrasting single, double, and four encryption quantum costs.

