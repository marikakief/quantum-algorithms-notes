# Review: Learning to predict arbitrary quantum processes (Huang-Chen-Preskill 2023)

Source note: [[Learning to predict arbitrary quantum processes (Huang-Chen-Preskill 2023) — Paper Notes]]

## Primary Sources Checked

- Huang, Chen, and Preskill, "Learning to predict arbitrary quantum processes", arXiv:2210.14894 / PRX Quantum 4, 040337 (2023).

## Verdict

Major issues. The summary is conceptually strong, but the opening target formula has a corrupted control character, and the note should foreground the input-distribution and observable restrictions even more.

## Line-Anchored Comments

- Lines 9-21: the problem statement is good, but line 12 contains corrupted LaTeX where `\bigl` should appear. Fix this because it is the first mathematical expression in the note.
- Lines 25-35: excellent explanation of the Heisenberg-picture move. Keep the warning that the process can be globally complex while the prediction task is locally/average-case learnable.
- Lines 43-44: the input-state set line is awkwardly split across a bare `$`. It may render, but it should be cleaned up.
- Lines 102-119: the theorem summary is good. Put "locally flat input distributions" and "bounded-degree observables" in the first sentence of the note's assessment, not only in the caveats.
- Lines 131-139: the Bohnenblust-Hille discussion is useful, but avoid implying that every bounded observable has a sparse Pauli expansion. The result is for local/bounded-degree structure.
- Lines 151-159: the caveats are excellent and should remain.

## Missing or Suggested Additions

- Add a one-sentence "not full process tomography" warning in the title section. The note says this later, but the paper title is easy to overread.
