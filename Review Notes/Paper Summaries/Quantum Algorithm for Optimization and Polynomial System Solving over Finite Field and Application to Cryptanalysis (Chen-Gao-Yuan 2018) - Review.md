# Review: Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018)

Source note: [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes]]

## Primary Sources Checked

- Chen, Gao, and Yuan, "Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis," arXiv:1802.03856.
- Ding, Gheorghiu, Gilyén, Hallgren, and Li, "Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems," as cited in the source note.

## Verdict

Minor issues. The note is appropriately skeptical and does not overstate the cryptanalytic implications. The main thing to preserve is that every polynomial runtime is conditional on sparse-access and condition-number assumptions inherited from the Macaulay/HHL subroutine.

## Line-Anchored Comments

- Lines 11-25: good to say "over `C`" because the finite-field problem is reduced to Boolean-complex polynomial solving before HHL enters.
- Lines 48-50: the assessment is exactly right. This is an attack template parameterized by `kappa`, not a break of SIS, SVP, CVP, or NTRU.
- Lines 56-67: the `theta_b` encoding is a useful reusable detail; it prevents illegal binary overflow values and should remain in the summary.
- Lines 86-104: the finite-field-to-Boolean reduction is clearly described. The surjective-not-injective warning at line 95 is important because it affects solution counting and conditioning intuition.
- Lines 132-154: the optimization by interval feasibility is well summarized. Mention that each feasibility query can have a different condition number; line 203 later handles this correctly.
- Lines 171-225: the theorem list is useful, but the displayed small exponents are potentially misleading unless every occurrence carries the `kappa^2` multiplier.
- Lines 237-249: excellent caveats. The 2023 Macaulay condition-number critique is not a formal refutation of every finite-field reduction, but it substantially weakens the cryptanalytic reading.

## Missing or Suggested Additions

- Add an explicit "oracle/access model" line near the beginning: sparse row/entry access to large derived Macaulay matrices is assumed, not constructed by materializing the matrices.
- If the note is used in a cryptography context, add a "no concrete resource estimate" tag or callout.

