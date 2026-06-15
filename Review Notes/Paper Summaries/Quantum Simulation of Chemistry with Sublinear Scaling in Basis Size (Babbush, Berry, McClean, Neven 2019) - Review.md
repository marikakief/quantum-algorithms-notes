# Review: Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019)

Source note: [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]]

## Primary Sources Checked

- Babbush, Berry, McClean, Neven, "Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size", arXiv:1807.09802 / npj Quantum Information 5, 92 (2019).
- Cross-checked with Su et al. 2021 for later compiled first-quantized costs.

## Verdict

Major issues. This note has several algebraic comparison errors. The algorithmic summary is good, but the scaling comparisons should be corrected before relying on the note.

## Line-Anchored Comments

- Line 31: the old/new ratio against Low-Wiebe, `N^{7/3}/eta^{10/3}`, is correct. The phrase "enormous in the regime `N >> eta`" is too loose: if `N = c eta`, the ratio scales as `c^{7/3}/eta`, so it can shrink with electron count unless the basis-per-electron ratio grows.
- Line 33: the norm statement is correct after substituting `Omega proportional to eta`, but the note should consistently distinguish `||T||` from `lambda_B`. The interaction-picture cost depends on the LCU norm of `B`, not the operator norm of `B`.
- Line 35: the CI comparison ratio is wrong. `eta^2 N^3 / (N^{1/3} eta^{8/3}) = N^{8/3} / eta^{2/3}`, not `eta^{2/3} N^{8/3}`. The subsequent `N = 100 eta` arithmetic is therefore also wrong.
- Line 55: "`||T|| >> ||B||` when `N >> eta`" is not what follows from the displayed scalings. Comparing `||T|| ~ N^{2/3}/eta^{1/3}` with `lambda_B ~ eta^{5/3}N^{1/3}` gives dominance only when `N >> eta^6`. The paper's improvement does not require selling this as a simple `N >> eta` norm dominance claim.
- Lines 71-76: the sign-parity cancellation explanation is useful. It should explicitly say these are unitary extensions whose invalid-grid contributions cancel in the LCU sum.
- Lines 123-129: the dominance condition for the qubitization alternative is wrong. Comparing `eta^{4/3}N^{2/3}` and `eta^{8/3}N^{1/3}`, the first term dominates when `N > eta^4`, not when `N << eta^{1/3}`.
- Lines 172-174: line 172 correctly repairs the Low-Wiebe ratio arithmetic. Line 174 correctly solves the CI crossover as `N = eta^{1/4}`. This conflicts with line 35; keep the later corrected version and remove the earlier one.
- Line 188: "No Born-Oppenheimer approximation required" is too strong for this paper's concrete Hamiltonian. The algorithmic representation can be extended to include nuclear degrees of freedom, but the paper as summarized is still about the Born-Oppenheimer electronic Hamiltonian with fixed nuclei.

## Missing or Suggested Additions

- Add a short "do not compare these monomials without specifying which quantity is fixed" warning. Several ratios change qualitatively depending on whether `eta`, `N/eta`, density, or grid spacing is held fixed.
- Since the follow-up Su et al. 2021 note exists, mark which claims are asymptotic-only and which have later constant-factor support.

