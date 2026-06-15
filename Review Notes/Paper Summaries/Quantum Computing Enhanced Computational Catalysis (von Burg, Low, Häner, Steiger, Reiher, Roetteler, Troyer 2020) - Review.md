# Review: Quantum Computing Enhanced Computational Catalysis (von Burg-Low-Haner-Steiger-Reiher-Roetteler-Troyer 2020/2021)

Source note: [[Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2007.14460
- Physical Review Research DOI linked in the source note.

## Verdict

Minor issues. The note gives a clear account of double factorization and the resource-estimation pipeline. The main fixes are publication-year metadata and precision around active-space/dynamical-correlation claims.

## Line-Anchored Comments

- Line 1: The arXiv preprint is 2020, but the journal publication is Physical Review Research 3, 033055 (2021). The filename says 2020; the note should state "2020 preprint / 2021 journal article."

- Lines 21-24: Good high-level summary. "Introduced double factorization as the algorithmic basis" should be phrased carefully: double factorization existed as a chemistry/tensor-decomposition idea, while this paper made it central to qubitized resource estimates for catalysis.

- Lines 31-37: The DF decomposition is described well. Add that truncating leaves changes both the Hamiltonian approximation error and the 1-norm `lambda`, so the threshold is an error-budget variable, not a purely numerical compression knob.

- Lines 41-45: The qubitization/QPE iteration count should specify the phase convention. Different block-encoding normalizations lead to `arcsin` versus `arccos` walk phases and slightly different constants.

- Lines 60-72: The FeMoCo and Ru estimates are useful. Keep the active-space size, target accuracy, and surface-code assumptions beside every number; otherwise comparisons across papers are easy to misuse.

- Lines 74-76 and 95-99: The note correctly identifies dynamical correlation as the central bottleneck. This should be in the headline verdict, because it is the main scientific limitation of the "computational catalysis" promise.

- Lines 105-115: Good reusable-ideas section. Distinguish "Double Factorization of Two-Electron Integrals" from "Double Factorisation for Block-Encoding"; one is classical factorization, the other is the quantum access construction.

## Missing or Suggested Additions

- Add a short comparison of sparse, single-factorized, double-factorized, and THC Hamiltonian representations in terms of `lambda` and oracle cost.

- Add an explicit warning that reducing Toffoli count for a fixed active space does not solve basis-set convergence or dynamic-correlation completeness.

