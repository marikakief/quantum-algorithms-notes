# Review: Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021)

Source note: [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2105.02859
- PRX Quantum DOI linked in the source note.

## Verdict

Minor issues. The note correctly treats the paper as a tutorial/unification rather than the original QSVT theorem. Most corrections are precision issues around QSP feasibility conditions, convention dependence, and a table entry that drops the sharper Hamiltonian-simulation precision term.

## Line-Anchored Comments

- Lines 7-13: Good high-level positioning. "No new results" is a little too blunt: the main contribution is pedagogical and organizational, but the QET presentation and worked unification are nontrivial exposition.

- Lines 54-60: The QSP achievability statement is incomplete unless the convention-specific parity, degree, boundedness, and completion conditions are all included. The displayed `P,Q` equation is one way to state the SU(2) completion, but the parity of `Q` and the real/complex convention should be stated.

- Lines 90-98: The QET explanation is clear. Add the normalization factor `H/alpha` whenever discussing eigenvalues in the signal interval; otherwise readers may forget that QET transforms normalized eigenvalues.

- Lines 111-114: The QSVT circuit written here is one standard convention, not "the" circuit. Left/right projector order, final `U_A` versus `U_A^\dagger`, and phase indexing differ across papers.

- Lines 170-181: The QSVT phase-estimation construction is a nice inclusion, but the threshold/gap and controlled-power costs should be explicit. The sign polynomial's degree is controlled by the promised separation from the threshold, not by bit index alone.

- Lines 187 and 212: These conflict. Line 187 correctly gives the Jacobi-Anger/QSP precision term `log(1/epsilon)/loglog(1/epsilon)` for Hamiltonian simulation; the table at line 212 simplifies it to `O(t + log(1/epsilon))`. Keep the sharper term in the table or label it as a coarse simplification.

## Missing or Suggested Additions

- Add a small "original theorem provenance" box: Low-Chuang for QSP/qubitization simulation, Gilyen-Su-Low-Wiebe for QSVT, Martyn-Rossi-Tan-Chuang for the tutorial/QET exposition.

- Mention that phase-factor synthesis is a separate classical preprocessing problem; the tutorial uses compiled phases but does not solve the numerical phase-finding problem.

