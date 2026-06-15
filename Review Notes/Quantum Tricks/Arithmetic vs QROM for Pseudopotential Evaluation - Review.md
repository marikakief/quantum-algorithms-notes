# Review: Arithmetic vs QROM for Pseudopotential Evaluation

Source note: [[Arithmetic vs QROM for Pseudopotential Evaluation]]

## Primary Sources Checked

- Berry et al., arXiv:2312.07654.
- Shokrian Zini et al. 2023 as described through the Berry et al. comparison.

## Verdict

Minor issues. The design principle is well stated. The "exponential improvement" wording should be made precise, and the lambda tradeoff should be moved into the headline.

## Line-Anchored Comments

- Line 7: "roughly 100x improvement" should be tied to the paper's block-encoding comparisons, not presented as a universal arithmetic-over-QROM constant.
- Lines 13-17: the contrast between tabulating over `N` basis states and computing analytic functions with `polylog N` arithmetic is the right central point.
- Line 17: "exponential improvement over `O(N)`" means exponential in the number of address bits `log N`, not exponential in the physical input size in the complexity-theory sense. Say this explicitly.
- Lines 19-22: the three enabling conditions are excellent. This is the part future notes should reuse.
- Lines 33-38: the table is useful but simplified. QROM costs can be space-time traded, and arithmetic costs include precision and function-interval constants.
- Lines 42-44: the lambda-inflation caveat is crucial. It belongs in "What it does", because the whole method trades cheaper SELECT/arithmetic for larger normalization and therefore more QPE steps.

## Missing or Suggested Additions

- Add a one-line criterion: arithmetic wins when analytic structure and moderate precision make `poly(b)` cheaper than table size after QROM/QROAM tradeoffs, and when the induced `lambda` increase is tolerable.

