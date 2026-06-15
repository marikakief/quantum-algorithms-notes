# Review: Shadow Tomography of Quantum States (Aaronson 2018)

Source note: [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]

## Verdict

Major issues. The conceptual description is good, but the main theorem statement conflates Aaronson's STOC 2018 bound with later improvements. Since this note is a central reference for the vault's shadow-tomography cluster, the epsilon dependence and computational-efficiency claims need to be exact.

## Line-Anchored Comments

- Lines 21-27: The note states an `epsilon^-4` copy bound as what "Aaronson gives." Aaronson's original shadow-tomography theorem is usually quoted with `tilde O(log^4 M log D / epsilon^5)` copies. Later work improved parameters and runtime models; keep those in a separate "subsequent improvements" sentence.

- Lines 53-59: The resource-accounting paragraph correctly mentions the original `epsilon^-5` total, which conflicts with the theorem statement at lines 21-27 and 63-66. Resolve this inconsistency.

- Lines 37-51: The algorithm sketch is good, but the update step should be described as maintaining a hypothesis through a postselected-learning/multiplicative-weights construction. It is not just an ordinary classical multiplicative-weights update on a compact object.

- Lines 63-69: The corollary for polynomial-size circuits is fine, but add that the measurements must be known in advance and that the procedure uses joint measurements across multiple copies.

- Lines 87-98: "Brandao et al. 2019: same sample complexity; poly(M,D) runtime" needs a precise citation and theorem statement. Different follow-ups trade copy complexity, running time, and measurement type.

- Lines 101-112: Good distinction between Aaronson shadow tomography and classical shadows. Add that classical shadows are not a direct replacement for arbitrary two-outcome measurements; the observable family and measurement ensemble determine the shadow norm.

## Missing or Suggested Additions

- Add a small historical table: Aaronson 2006 learnability, Aaronson 2018 shadow tomography, Brandao-Harrow-Oppenheim-Strelchuk/Gibbs-state variants, Huang-Kueng-Preskill classical shadows.
- State the measurement model in every comparison: collective/adaptive, few-copy, or single-copy randomized measurements.
