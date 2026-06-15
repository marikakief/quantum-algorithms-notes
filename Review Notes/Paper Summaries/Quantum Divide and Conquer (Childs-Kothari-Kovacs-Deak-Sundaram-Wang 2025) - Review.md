# Review: Quantum Divide and Conquer (Childs-Kothari-Kovacs-Deak-Sundaram-Wang 2025)

Source note: [[Quantum Divide and Conquer (Childs-Kothari-Kovacs-Deak-Sundaram-Wang 2025) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2210.06419
- ACM TQC metadata linked from arXiv: DOI https://doi.org/10.1145/3723884

## Verdict

Major issues in the strength of the optimality claims, though the broad idea is right. The source abstract says the framework applies "in certain cases" and gives near-optimal query complexities for specific string problems. The note repeatedly states a more universal recurrence and more universal tightness than the paper supports.

## Line-Anchored Comments

- Lines 19-23: The recurrence `C_Q(n) <= sqrt(a) C_Q(n/b) + O(C_aux)` should be introduced as a framework that applies under specific structural hypotheses, not as a general conversion of classical divide-and-conquer algorithms.

- Line 22: The general adversary bound/span-program correspondence is not due simply to Spalek-Szegedy. Spalek-Szegedy is important for adversary-method equivalences, but the tight algorithmic characterization by span programs is associated with Reichardt and subsequent work. Fix this attribution.

- Lines 24 and 64-76: The claim that the speedups are not reachable by Grover/amplitude amplification/walks "off the shelf" is plausible as commentary, but it is not a theorem. Label it as interpretation.

- Lines 30-34: "Orthogonally combined witnesses" is suggestive but underdefined. The paper's conditions should be stated more concretely if this is meant to be a reusable technical summary.

- Lines 38-47: The applications are too compressed. For LIS/LCS the note should state the parameterized decision version and its parameter dependence, not merely "improved query complexity."

- Lines 52-60: "Tightness" is overstated. The arXiv abstract says "near-optimal" for various string problems; do not state that all obtained complexities are essentially optimal unless the matching lower bound and parameters are shown for each problem.

- Lines 82-86: Good caveats. Add one more: this is query complexity, and the span-program-to-algorithm conversion can hide implementation overheads not captured by the recurrence.

## Missing or Suggested Additions

- Add a table with the actual query complexities for regular languages, string rotation, string suffix, LIS, and LCS. Without the exponents/parameters, the summary is too slogan-like.

- Add a "not a master theorem" warning: the recurrence is an output of adversary/span-program structure, not a plug-in replacement for every classical recurrence.

