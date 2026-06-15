# Review: Truncated Taylor Series Simulation

Source note: [[Truncated Taylor Series Simulation]]

## Primary Sources Checked

- BCCKS truncated Taylor, arXiv:1412.4687: https://arxiv.org/abs/1412.4687
- Low-Chuang qubitization, arXiv:1610.06546, for later comparison: https://arxiv.org/abs/1610.06546

## Verdict

Needs rewrite. The card mixes correct Taylor-LCU intuition with incorrect coefficient preparation, wrong/loose complexity statements, and misleading source metadata.

## Line-Anchored Comments

- Line 2: the source should be the 2015 PRL/arXiv:1412.4687 paper. Calling it "FOCS 2015 (original)" conflates it with the different Berry-Childs-Kothari nearly-optimal paper.
- Line 7: precision scaling is not just `O(log 1/epsilon)`; the truncation order and query count carry `log(T/epsilon)/loglog(T/epsilon)`.
- Line 17: `K = O(log(1/epsilon))` is a coarse shorthand and should not be the main formula. Use the sharper `log(T/epsilon)/loglog(T/epsilon)` expression.
- Line 24: the PREPARE amplitudes cannot contain `sqrt((-i lambda t)^k ...)`. Prepare nonnegative magnitudes; put the `(-i)^k` phase into SELECT or the controlled branch.
- Line 31: without segmentation, OAA repeated `O(Lambda)` times is not the truncated-Taylor algorithm used for optimal scaling. The algorithm chooses segments so the coefficient sum per segment is near 2, then uses single-round robust OAA.
- Lines 33-36 and 57-60: the total cost should use `K = O(log(Lambda t/epsilon)/loglog(Lambda t/epsilon))`, not a plain logarithm.
- Line 57: the ancilla count `O(log L + log K)` is too small for the straightforward BCCKS register layout, which stores up to `K` term labels. The paper-note version's `O(K log L)` is safer unless a compressed implementation is being specified.
- Line 65: success probability is probability `1/s^2` for amplitude prefactor `1/s`, not `1/Lambda` as written.
- Line 67: "superseded ... by a log factor" should be phrased as a query-model asymptotic comparison. Taylor methods remain useful pedagogically and sometimes practically.

## Missing or Suggested Additions

- Split the card into two cases: naive whole-interval Taylor LCU and the BCCKS segmented `ln 2` algorithm. The current version blends them.
