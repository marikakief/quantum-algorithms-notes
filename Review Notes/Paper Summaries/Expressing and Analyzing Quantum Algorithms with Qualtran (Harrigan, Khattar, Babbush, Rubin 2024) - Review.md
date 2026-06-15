# Review: Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan-Khattar-Yuan-Peduri-Yosri-Malone-Babbush-Rubin 2024)

Source note: [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2409.04643
- Project repository: https://github.com/quantumlib/Qualtran

## Verdict

Minor issues. This is a detailed and useful note. The technical content is mostly accurate, but a software/resource-estimation paper needs extra version control: primitive costs, physical cost models, and library APIs can change quickly.

## Comments

- The filename/title shortens the author list. The source note has the full author list; keep the full citation in the body or metadata so searches do not miss Yuan, Peduri, Yosri, and Malone.

- The note correctly says this is not a new algorithm paper. Keep that visible, because several tables could otherwise read like Qualtran is the source of the underlying QROM, QSVT, chemistry, or cryptography algorithms.

- Gate-count tables should be labeled as Qualtran's implementation at the paper/version checked, not immutable constants. Comparator, swap, measurement-uncomputation, and rotation-synthesis choices are active implementation details.

- The qubit-counting discussion is good. It should emphasize that hierarchical min-max qubit accounting is an estimator, not a guarantee of an optimized compiled layout.

- Physical-resource estimates are surface-code-model dependent. The note should avoid language suggesting architecture-independent physical costs; the architecture-independent layer is the logical resource count.

- The tensor/classical simulation paragraphs are useful, but they are validation tools for small or reversible subroutines, not evidence that large fault-tolerant algorithms can be classically simulated.

## Suggested Additions

- Add a "version checked" line with the arXiv version and repository commit if possible.
- Add a table separating algorithmic bloqs, logical-cost protocols, and physical-cost models.
