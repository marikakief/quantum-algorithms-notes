# Review: An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026)

Source note: [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes]]

## Primary Sources Checked

- Anshu, "An alternating-minimization method for preparing low-energy states," arXiv:2603.15495 (2026).
- arXiv metadata checked May 26, 2026; the preprint was submitted March 16, 2026.

## Verdict

Minor issues. The note is unusually candid about the heuristic status of the algorithm and correctly emphasizes the energy-dependent uncertainty principle. The main improvement is to move the "variance is not progress" caveat closer to the headline claims.

## Line-Anchored Comments

- Line 9: "existing methods get stuck" should be softened to "can get stuck." The paper is diagnosing a real failure mode, not claiming every existing method typically stalls this way.
- Lines 13-15: the assessment is good. Add immediately here that high variance with respect to altered Hamiltonians is only a proxy for possible progress; it does not by itself prove useful spectral weight below the current energy.
- Lines 13 and 23-29: "share the same low-energy subspace" is exact for zero-energy ground spaces of frustration-free projector Hamiltonians. For low-frustration or frustrated cases, the relationship is more heuristic/approximate and should be stated that way.
- Lines 33-41: the local uncertainty-principle summary is clear. The dependence hidden inside `Omega(1)` may depend on local ranks/normalization; make sure this is consistent with the paper's assumptions before turning it into a trick card.
- Line 45: verify whether the Gaussian parameter is variance or standard deviation. The phrase "variance `sqrt(E_beta/t_beta)`" is dimensionally suspicious.
- Lines 57-63: the copy count is well stated and important. Keep it visible in any short summary of the energy-measurement variant.
- Lines 65-74: the variational variant should mention measurement cost for estimating the commutator signs. The method assumes access to good estimates of these local quantities.
- Lines 92-102: the numerical table is useful. Since the paper is from 2026 and the examples are small, mark these as exploratory rather than evidence of scaling.
- Lines 121-131: the caveat section is strong. The point on line 125 is central enough to repeat in the opening: variance does not imply a quartile-energy improvement.

## Missing or Suggested Additions

- Add a one-line complexity-status label near the top: "Theorem: uncertainty principle; heuristic: alternating descent; evidence: small numerics." That will prevent readers from upgrading the algorithmic proposal into a convergence theorem.

