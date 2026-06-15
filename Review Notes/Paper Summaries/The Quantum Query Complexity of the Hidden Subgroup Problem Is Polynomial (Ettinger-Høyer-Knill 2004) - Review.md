# Review: The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004)

Source note: [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]

## Primary Sources Checked

- Ettinger, Høyer, and Knill, "The quantum query complexity of the hidden subgroup problem is polynomial", arXiv:quant-ph/0401083 / Information Processing Letters 91 (2004).

## Verdict

Minor issues. The note captures the query-versus-computation separation well. The only substantive concern is a cross-link phrase that suggests solvable-group HSP is broadly solved; Watrous solves important solvable-group structural tasks, not arbitrary HSP over every solvable group.

## Line-Anchored Comments

- Lines 9-11: good problem setup. The subgroup-count bound is the reason `O(log^4 |G|)` queries are enough after exactification.
- Lines 15-21: excellent statement of the result's meaning. Query lower bounds cannot explain nonabelian HSP hardness.
- Lines 31-53: the sequential projector test is accurately summarized. The decreasing-size order is essential and should remain.
- Lines 55-63: exactification is correctly framed as query complexity, not an efficient algorithm. Mention that the probability matrix and rotations are exponentially expensive to compute/implement.
- Lines 67-73: theorem/corollary statements are useful.
- Lines 77-93: the query-computation table is good. "Never the bottleneck" means asymptotically in query complexity, not in concrete oracle-cost models.
- Lines 95-100: caveats are strong. The exact-gate caveat is important in a finite-gate-set model.
- Line 130: revise "efficient HSP for solvable groups" to "quantum algorithms for solvable-group structural problems" unless a specific normal-HSP result is intended.

## Missing or Suggested Additions

- Add one sentence distinguishing copies of hidden-subgroup states from an efficiently implementable pretty-good measurement on those copies.

