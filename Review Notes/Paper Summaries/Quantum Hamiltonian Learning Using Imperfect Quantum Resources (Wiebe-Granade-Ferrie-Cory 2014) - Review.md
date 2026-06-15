# Review: Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014)

Source note: [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]

## Primary Sources Checked

- Wiebe, Granade, Ferrie, and Cory, "Quantum Hamiltonian Learning Using Imperfect Quantum Resources," Physical Review A 89, 042314 (2014) / arXiv:1311.5269.

## Verdict

Minor issues, with one notation problem. The note gives a good account of the robustness sequel to QHL, but the depolarizing-noise parameter is described inconsistently.

## Line-Anchored Comments

- Lines 7-15: the problem framing is good. Keep the distinction between imperfect quantum resources, model mismatch, and model selection; they are different robustness questions.
- Lines 21-29: the summary is accurate in spirit. "Even 50% depolarization still works" should specify the simulated setting and the fact that learning slows rather than keeping the same constant factors.
- Lines 49-63: notation issue. The text calls `N` a "visibility" or preserved coherent fraction, but the displayed state uses `(1-N)` on the coherent component and `N/2^n` on the maximally mixed component. Either rename it as a depolarizing probability/noise fraction or reverse the prose.
- Lines 65-69: the learning-rate rescaling is an approximate/specific-model statement. Do not present it as a universal theorem for arbitrary noise.
- Lines 71-81: the superoperator incorporation paragraph is good. Add that including a calibrated noise channel in the likelihood only helps if that calibration remains stable during learning.
- Lines 100-112: the model-mismatch section is valuable. The saturation floor should be framed as "best model in the supplied class," not as discovery of the true Hamiltonian.
- Lines 116-120: "for free" model selection is fine only in the sense that SMC already computes normalizers; one still runs and maintains separate filters for the competing models.
- Lines 137-149: the caveats are strong and should remain.

## Missing or Suggested Additions

- Add a small notation table for the depolarizing parameter and visibility. This paper is easy to misread because "visibility" and "noise probability" are complements.

