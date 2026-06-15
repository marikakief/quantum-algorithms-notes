# Review: Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025)

Source note: [[Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025) — Paper Notes]]

## Primary Sources Checked

- Khattar, Shutty, Gidney, Zalcman, Yosri, Maslov, Babbush, and Jordan, "Verifiable Quantum Advantage via Optimized DQI Circuits," arXiv:2510.10967 (2025).

## Verdict

Minor issues. The note is careful about best-known classical attacks and physical assumptions. The main improvement is to keep "verifiable advantage candidate" distinct from a proved lower bound or an achieved hardware demonstration.

## Line-Anchored Comments

- Lines 29-35: strong summary. Keep the sentence saying this is a best-known-algorithm statement, not an unconditional lower bound.
- Lines 41-50: the lower-qubit DQI circuit explanation is clear and useful. It should mention that measurement-based uncomputation is still a fault-tolerant feed-forward/control-flow assumption in the resource model.
- Lines 74-81: good EEA tradeoff table. "Best use here" should be labeled as parameter-regime-dependent, which line 158 later does.
- Lines 83-101: the XP discussion is one of the best parts of the note. Keep the target-set dependence visible; otherwise the binary-extension-field improvement sounds too automatic.
- Lines 105-107: excellent self-correction. This should be repeated in the headline or abstract.
- Lines 118 and 153: XP lower bounds are lower bounds against the XP attack model, not all classical algorithms.
- Lines 122-127: the distinction between structured objectives and arbitrary target sets is essential. Keep it close to the asymptotic `tilde O(m)` theorem.
- Lines 129-138: the concrete resource estimates are useful but hardware-model-specific. Any future quote should include the surface-code/cultivation/yoked-storage assumptions.
- Lines 145-149: the comparison with Shor/Regev/Jacobi is interesting but should be kept clearly as context, not an equivalence of problem status.
- Lines 151-159: caveats are strong and should remain.

## Missing or Suggested Additions

- Add a one-line status label: "Candidate verifiable quantum advantage based on best-known classical attacks; not a demonstrated advantage and not an unconditional classical lower bound."

