# Review: Oblivious Amplitude Amplification (Robust)

Source note: [[Oblivious Amplitude Amplification (Robust)]]

## Primary Sources Checked

- BCCKS 2014, arXiv:1312.1414: https://arxiv.org/abs/1312.1414
- BCCKS truncated Taylor, arXiv:1412.4687: https://arxiv.org/abs/1412.4687

## Verdict

Sound with minor caveats. The card explains the 2D-subspace idea well, but should state the hypotheses more prominently.

## Line-Anchored Comments

- Lines 8-16: the formula assumes the success amplitude is the same for every input state and that `V` is unitary. Those are the conditions that make the amplification genuinely "oblivious."
- Line 20: "exact amplification with 3 queries" should say three uses of `U`/`U^\dagger` in the standard single-round construction.
- Lines 26-33: the 2D-subspace proof is strong and should be preserved.
- Lines 41-44: robust OAA depends on the target being close to a unitary and `s` being close to 2. State these hypotheses before the displayed bound.
- Line 58: the general `O(1/sqrt p)` statement is ordinary amplitude-amplification scaling; exact deterministic success requires phase adjustment or special angles.
- Line 61: this caveat is good; consider moving it earlier so users do not overapply OAA.

## Missing or Suggested Additions

- Add a "do not use when" sentence: if the top-left block is a genuinely nonunitary contraction and not close to a unitary, use QSVT/block-encoding methods rather than assuming OAA makes it deterministic.
