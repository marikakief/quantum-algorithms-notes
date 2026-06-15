# Review: Order-Condition Cancellation in Product Formulas

Source note: [[Order-Condition Cancellation in Product Formulas]]

## Primary Sources Checked

- Childs, Su, Tran, Wiebe, Zhu, "A Theory of Trotter Error," arXiv:1912.08854: https://arxiv.org/abs/1912.08854

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Line 8: good intuition, but phrase this as aggregate cancellation of the expansion through order `p`, not as every individual Taylor or commutator monomial separately becoming zero.
- Line 12: this is the right warning for students: do not add cancelled low-order terms as if they were independent errors.
- Line 14: the commutator-scaling pointer is useful, but it needs to be paired with the step-count/global-error relation from the companion trick card.
- Lines 22-24: the caveat about coefficient correctness and high-order stability is important and should be kept.

## Missing or Suggested Additions

- Add one explicit example: Strang splitting cancels the second-order local error term, so the leading local error is third order.
