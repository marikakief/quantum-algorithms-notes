# Review: Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016)

Source note: [[Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) — Paper Notes]]

## Primary Sources Checked

- Harrow, Lin, and Montanaro, "Sequential measurements, disturbance and property testing", SODA 2017 / arXiv:1607.03236.

## Verdict

Minor issues. The note is careful and captures the measurement-disturbance primitive well. The main improvements are to add a title, keep query-efficient versus time-efficient caveats close to the applications, and avoid making the one-copy OR primitive sound stronger than its yes/no promise.

## Line-Anchored Comments

- Add a top-level title.
- Lines 9-17: good motivation via noncommuting measurements and anti-Zeno drift.
- Lines 21-31: the yes/no promise should be stated with the same thresholds as the theorem. In particular, the yes case is near-certain acceptance by one measurement; the no case is small average acceptance.
- Lines 33-41: useful application list. Add "query/copy complexity" to the lead-in so readers do not infer efficient time implementations for arbitrary groups or measurement families.
- Lines 79-105: Theorem 9 and Corollary 11 are summarized well.
- Lines 107-121: good invariant-state test summary.
- Lines 145-158: the function-isomorphism applications are clear. Keep the property-testing promise visible for graph isomorphism.
- Lines 210-217: the de-Merlinization correction is important and accurately motivated.
- Lines 239-242: strong caveats. These should be moved earlier if the note is used pedagogically.

## Missing or Suggested Additions

- Add a short "what this does not give" paragraph: it is not a general procedure for trying exponentially many tests in polynomial time unless the averaged measurement can be implemented efficiently.

