# Review: Quantum Walks and Their Algorithmic Applications (Ambainis 2003)

Source note: [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]]

## Primary Sources Checked

- Ambainis, "Quantum walks and their algorithmic applications", arXiv: https://arxiv.org/abs/quant-ph/0403120
- Ambainis element-distinctness paper for the Johnson-graph details: https://arxiv.org/abs/quant-ph/0311001

## Verdict

Major issues. The survey summary is useful, but it overstates quantum-walk search as a generic `O(sqrt(N/M))` graph-search rule.

## Line-Anchored Comments

- Lines 7-14: survey role

  Good. This is a compact survey/overview, not a single theorem paper introducing all of the machinery from scratch.

- Lines 44-56: glued trees

  Good caveat: the speedup is in a graph-oracle/local-exploration model. If the graph labeling exposes the structure, the classical problem changes.

- Lines 70-79: grid-search table

  High-risk. The dimensional thresholds and log factors depend on continuous versus discrete walk model, success-probability amplification, and the specific coin. If retained, cite the original Childs-Goldstone and Ambainis-Kempe-Rivosh results directly.

- Lines 81-93: element distinctness

  Good. The Johnson-graph explanation is the clearest part of the note.

- Lines 95-98: generalizations

  These are survey-level statements. `k`-clique/subgraph finding is not merely "via reduction to k-element distinctness" in a black-box way; the graph data structure and checking cost matter.

- Lines 102-113: "common structure" / `O(sqrt(N/M))`

  Too broad. Quantum walk search on graphs is not automatically Grover search over vertices. The cost depends on spectral gap, marked stationary measure, update/checking costs, and sometimes graph symmetry. Replace this with a pointer to Szegedy/MNRS-style `S + (1/sqrt(epsilon))((1/sqrt(delta))U + C)`.

- Line 134: wrong link target for Grover

  The link text says Grover but points to the BHMT amplitude-amplification note. Link to `[[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]`.

## Missing or Suggested Additions

- Add a "survey caveat" near the top: the paper is a map of several walk paradigms; claims in this note should not be used as exact cost formulas without the cited application paper.
