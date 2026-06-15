# Review: Quantum Lower Bounds for Simulating Fluid Dynamics (Ameri-Carolan-Childs-Krovi 2026)

Source note: [[Quantum Lower Bounds for Simulating Fluid Dynamics (Ameri-Carolan-Childs-Krovi 2026) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2603.12161

## Verdict

Major issues from metadata errors and overbroad interpretation. The high-level lower-bound message is right, but the author list in the source note is wrong, and the implications for "quantum advantage impossible" need to be stated within the copy-complexity/final-state-preparation model.

## Line-Anchored Comments

- Line 1: The arXiv page lists the authors as Abtin Ameri, Joseph Carolan, Andrew M. Childs, and Hari Krovi. The note says "Euan Carolan" and "Nolan Krovi"; both should be corrected.

- Lines 20-27: The lower-bound framing is good. Add that the bounds apply to preparing the final state, with similar bounds for history-state preparation, as stated in the arXiv abstract.

- Lines 34-43: The degree-`k` polynomial-in-density-matrix intuition is useful, but it should be phrased carefully: the proof reduces copy access to polynomial dependence on input-state amplitudes/density entries for the relevant output task. Avoid making it sound like every quantum algorithm literally computes a polynomial map pointwise.

- Lines 46-48: These main theorem bullets match the arXiv abstract: `Omega(T^2)` copies for KdV and `e^{Omega(T)}` copies for Euler in the worst case.

- Lines 58-60: "Making exponential quantum speedup impossible" is too strong. The lower bounds constrain algorithms that need copies of an unknown initial state to prepare the final state. They do not rule out all quantum advantage for observable estimation, restricted initial data, classical-description input models, dissipative flows, or alternative output tasks.

- Lines 70-76: Good distinction that Navier-Stokes with viscosity is not covered. Keep this distinction near the headline, not only in the comparison table, because many readers will conflate Euler with practical CFD.

- Lines 87-91: These caveats are important and should be moved earlier. They are not afterthoughts; they define the scope of the theorem.

## Missing or Suggested Additions

- Add a version/status line: arXiv v1 was submitted March 12, 2026. This is a fresh preprint as of the current vault review date.

- Add a short explanation of the two proof ideas from the abstract: KdV uses divergence of solitons; Euler uses instabilities enabling fast state discrimination.

