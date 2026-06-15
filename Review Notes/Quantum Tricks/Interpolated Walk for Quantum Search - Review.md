# Review: Interpolated Walk for Quantum Search

Source note: [[Interpolated Walk for Quantum Search]]

## Primary Sources Checked

- Krovi, Magniez, Ozols, Roland, arXiv:1002.2419: https://arxiv.org/abs/1002.2419

## Verdict

Major issue: the optimal interpolation parameter is wrong. This is the same square-root error found in the linked paper summary.

## Line-Anchored Comments

- Line 7: "full quadratic speedup on any reversible Markov chain"

  Correct for a single marked element in terms of ordinary hitting time. For multiple marked elements the speedup is in terms of extended hitting time `HT^+`, which can be larger than `HT`.

- Lines 14-18: `s*` is wrong.

  From the overlap formulas in the paper summary, equal overlap requires `(1-s)(1-p_M)=p_M`, so `s* = 1 - p_M/(1-p_M)` when `p_M < 1/2`. The card's `1 - sqrt(p_M/(1-p_M))` does not balance `cos theta` and `sin theta`.

- Line 18: precision notation should distinguish bits from inverse precision.

  The paper often parameterizes eigenvalue estimation by `T=2^t`, where `t` is the number of bits/rounds. Saying "precision `T`" is fine only if `T` is defined as the inverse phase resolution.

- Line 22: 2D-grid example is good.

  Define whether `n` denotes the number of vertices or side length when importing this comparison into a standalone card.

- Line 27: complexity statement is right after fixing the `s*` parameter.

  Include setup cost overheads when `p_M` or `HT^+` is unknown.

## Missing or Suggested Additions

- Add the one-line derivation of `s*`; it is the easiest way to prevent the square-root mistake.
