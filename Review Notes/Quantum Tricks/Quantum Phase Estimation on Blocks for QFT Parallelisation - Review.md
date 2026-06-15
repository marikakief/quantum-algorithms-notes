# Review: Quantum Phase Estimation on Blocks for QFT Parallelisation

Source note: [[Quantum Phase Estimation on Blocks for QFT Parallelisation]]

## Primary Sources Checked

- Kahanamoku-Meyer et al., arXiv:2505.00701.

## Verdict

Minor issues. The card captures the core parallelization trick. It should fix a likely denominator inconsistency and add normalization caveats.

## Line-Anchored Comments

- Line 12: the inter-block phase denominator should be checked against the source. The paper-summary note later uses `/2^{2m}`, so this card should not use `/2^m` unless a different convention is defined.
- Lines 14-20: the QPE recovery story is good, but the displayed state is missing normalization. That is fine for intuition, but label it as schematic.
- Lines 22 and 40-42: the wraparound caveat is well explained and should remain.
- Lines 32-36: "gate range `O(m)`" is the correct locality statement; keep it consistent with the paper note.

## Missing or Suggested Additions

- Add a one-line definition of the Lee metric/wraparound distance because it is the heart of why ordinary arithmetic error can be large even when QPE is close modulo `2^m`.

