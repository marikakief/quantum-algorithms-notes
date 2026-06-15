# Review: Fast Quantum Algorithm for Differential Equations (Bagherimehrab-Nakaji-Wiebe-Brennen-Sanders-Aspuru-Guzik 2023)

Source note: [[Fast Quantum Algorithm for Differential Equations (Bagherimehrab-Nakaji-Wiebe-Brennen-Sanders-Aspuru-Guzik 2023) — Paper Notes]]

## Primary Sources Checked

- Bagherimehrab et al., "Fast quantum algorithm for differential equations", arXiv:2306.11802.

## Verdict

Minor issues. This is one of the better PDE notes: it states the structural promise, the wavelet preconditioner, and the observable-output caveat. The main correction is a dimension/condition-number convention issue.

## Line-Anchored Comments

- Lines 27 and 63: `N=2^{nd}` is the total number of grid points, but line 63 says a second-order discretisation has typical `kappa(A) ~ N^2`. That is true if `N` denotes the number of grid points per dimension; with total grid size `N`, the usual scaling is closer to `N^{2/d}`. Use one grid-size convention consistently.
- Lines 47-49: good caveat that this is not a generic linear-system condition-number escape. Keep this near every headline "polylog in grid size" claim.
- Lines 57-75: the wavelet transform discussion is clear. It would help to say whether the assumed block-encoding of the finite-difference matrix remains efficient after the basis change or whether the algorithm is counting the basis-change gates separately.
- Lines 85 and 191-209: the theorem statement should say exactly which constants the `O(1)` condition number depends on: wavelet family, ellipticity/coercivity constants, dimension assumptions, and boundary treatment.
- Lines 111-127: good explanation of why directly applying the diagonal preconditioner is not fast.
- Lines 151-163: the extended observable identity is an important point and is written clearly. Add that estimating the normalization factor `xi^2` is a separate statistical task.
- Lines 183-187 and 246-248: the boundary-condition caveat is good, but it is important enough to mention in the problem statement too.

## Missing or Suggested Additions

- Add a sentence contrasting this wavelet-preconditioning result with lower bounds for arbitrary QLSPs: the promise is PDE regularity/coercivity plus a known multiscale basis.
