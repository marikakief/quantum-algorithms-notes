# Review: Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013)

Source note: [[Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013) — Paper Notes]]

## Primary Sources Checked

- Kuperberg, "Another subexponential-time quantum algorithm for the dihedral hidden subgroup problem", arXiv:1112.3333 (2011 preprint; published 2013).

## Verdict

Minor issues. The note is brief but basically right. It should add the formal theorem/tradeoff parameters and avoid overstating "more useful" without specifying the resource model.

## Line-Anchored Comments

- Lines 1-13: metadata is good, but the note is missing a top-level title heading before the source block.
- Lines 30-38: the core summary is right: collimation keeps subexponential time while reducing quantum memory by using depth-first traversal and classical bookkeeping.
- Lines 43-59: the algorithmic structure is serviceable, but it needs one equation for collimation: tensor phase vectors, measure coefficient sums modulo a chosen power, and retain a narrower congruence class.
- Lines 66-69: the key results are true at a high level, but too vague. Add the paper's explicit time/space tradeoff form or at least define which model uses QRACM.
- Lines 74-76: "more useful" is defensible for resource accounting, but say "more useful for separating quantum memory from classical bookkeeping" rather than making a global ranking.
- Lines 79-83: the reusable ideas are good and should become trick cards if they do not already exist.

## Missing or Suggested Additions

- Add a comparison table against Kuperberg 2005 and Regev 2004: quantum space, classical space, query/time scale, and whether QRACM is assumed.
- Cross-reference the Kuperberg 2005 note's line claiming a cube-root-log improvement, because that older note needs correction to match this one.

