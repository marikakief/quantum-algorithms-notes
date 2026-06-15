# Review: Decoding Quantum Errors with Subspace Expansions (McClean-Jiang-Rubin-Babbush-Neven 2019)

Source note: [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]]

## Primary Sources Checked

- McClean, Jiang, Rubin, Babbush, and Neven, "Decoding quantum errors with subspace expansions," Nature Communications 11, 636 (2020) / arXiv:1903.05786.

## Verdict

Minor issues, with one technical correction in the stochastic-sampling paragraph. The note gives a good account of stabilizer projection as post-processed error mitigation and is properly cautious about fault tolerance.

## Line-Anchored Comments

- Lines 11-20: the summary is strong. Add that the denominator `Tr(P rho P)` is a postselection/signal factor; if it becomes small, the estimator variance can become large.
- Lines 24-36: the stabilizer-projection derivation is clear. It would help to explicitly say that measuring `Gamma_j M_k` can increase Pauli weight and therefore hardware/readout sensitivity.
- Lines 38-46: likely correction needed. The sampling probability cannot generally be `p_{chi,j} = gamma_j / 2^m` if Pauli coefficients `gamma_j` can be negative. It should be based on absolute weights with signs absorbed into the estimator.
- Line 46: "cost scales with the error rate, not with code size" is too strong. Under the toy variance model the additional variance is controlled by state quality, but code size still enters through Pauli weights, measurement implementation, and the number of sampled operator families.
- Lines 48-58: the projection-versus-recovery tradeoff is useful and should stay. It is a good explanation of why detection can remove more error patterns than recovery can correct.
- Lines 60-70: the QSE explanation is good. The "linear rather than quadratic" measurement-cost statement relies on commutation structure for the chosen code/check Hamiltonian; it should not be generalized to arbitrary QSE bases.
- Lines 72-80: the unencoded-symmetry application is well described. Distinguish exact symmetries, which are safe projectors, from approximate occupation checks, which are variational/error-mitigation heuristics.
- Lines 110-117: excellent caveats. The pseudo-threshold warning is essential and should remain near the headline `p approx 0.50` claim.

## Missing or Suggested Additions

- Add a short "postselection denominator" warning: projection-based mitigation improves bias only when enough state weight remains in the projected subspace to estimate ratios reliably.

