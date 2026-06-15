# Review: Worst-to-Average-Case Reduction for Optimistic Circuits

Source note: [[Worst-to-Average-Case Reduction for Optimistic Circuits]]

## Primary Sources Checked

- Kahanamoku-Meyer et al., arXiv:2505.00701.

## Verdict

Minor issues. The reduction is accurately described. The randomized versus purified guarantees need sharper labels.

## Line-Anchored Comments

- Lines 12-22: the sandwiching idea is clear.
- Line 18: "maps any input state to an effectively random state" is informal. A 1-design is sufficient for the expected squared-error calculation; do not imply full Haar randomness.
- Lines 24-26: the purification version is useful. Say explicitly that it changes the input state by adding a control register and gives a coherent derandomized implementation, not the same circuit on the original register alone.
- Lines 36-44: good caveats. Keep the warning that `U V^\dagger U^\dagger` may be hard for arbitrary `U`.

## Missing or Suggested Additions

- Add the distinction between expected error over classical random choice of `V` and deterministic coherent error after purification.

