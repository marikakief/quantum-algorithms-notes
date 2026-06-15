# Review: Quantum Linear Matrix Equations

Source note: [[Quantum Linear Matrix Equations — Paper Notes]]

## Primary Sources Checked

- Somma, Low, Berry, and Babbush, "Quantum algorithm for linear matrix equations", arXiv:2508.02822.

## Verdict

Major omissions. The operator-output viewpoint is important and the note captures it well, but this is too high-level to function as a reliable paper summary. It needs the actual theorem statements, complexity parameters, and assumptions.

## Line-Anchored Comments

- Add a top-level title. The file currently starts with a blank line and metadata.
- Lines 8-18: good central takeaway: block-encoding `X` is different from preparing `|X>>`.
- Lines 22-31: the vectorization comparison is useful. Add the precise cost or normalization problem caused by vectorization.
- Lines 35-39: "sandwich LCU" is a good phrase, but the note should define the normalization and success probability of the resulting block encoding.
- Lines 49-53: the transform-tool list is too vague. It names LCHS and Hubbard-Stratonovich machinery but gives no theorem conditions for when each is used.
- Lines 62-66: the BQP-completeness claim is vague. State what problem variant is BQP-complete and under what reductions. "Not efficiently classifiable classically" is not a standard complexity-theoretic conclusion.
- Lines 70-76: the follow-up connection is current and useful, but it should not substitute for summarizing this paper's own main theorems.
- Lines 78-83: good caveats. Add the actual condition-number and normalization symbols from the paper.

## Missing or Suggested Additions

- Add a theorem table: assumptions on `A,B,C`, output block-encoding normalization, query complexity, and dependence on the effective Sylvester condition number.
- Add one worked distinction between solving `AX+XB=C` as an operator problem and solving `(A \otimes I + I \otimes B^T)|X>>=|C>>` as a state problem.

