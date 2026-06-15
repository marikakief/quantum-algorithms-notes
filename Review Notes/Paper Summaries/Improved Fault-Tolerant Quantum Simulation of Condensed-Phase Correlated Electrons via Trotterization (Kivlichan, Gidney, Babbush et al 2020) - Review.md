# Review: Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020)

Source note: [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]]

## Primary Sources Checked

- Kivlichan et al., "Improved fault-tolerant quantum simulation of condensed-phase correlated electrons via Trotterization", arXiv:1902.10673 / Quantum 4, 296 (2020).

## Verdict

Minor issues. This is a strong note, especially on extensive versus intensive error. The fixes are mostly about not overstating the non-perturbative error bound and finite-size resource comparison.

## Line-Anchored Comments

- Lines 15-19: the headline comparison to LCU/qubitization is acceptable only with the extensive-error convention and reported finite-size compilation assumptions. Add that caveat early, not only later.
- Lines 38-39: the displayed second-order Trotter formula reads like a single step. Add the repetition/time-slicing parameter so readers do not confuse one symmetric product formula with the full evolution.
- Lines 60-65: "non-perturbative" is too broad as written. The theorem avoids a short-time series expansion in the desired way, but the note later records a condition such as `Wt^3 <= sqrt(2)`. Mention that condition next to the first claim.
- Lines 83-95: the extensive/intensive discussion is excellent and should be retained. This is the part most students miss.
- Line 125: "Campbell row" should specify whether the comparison is against a compiled finite-resource estimate or against an asymptotic expression imported from another setting.
- Lines 136-139: the observation that the asymptotic crossover is far beyond practical `N` is important. Move a compact version of it into the summary.

## Missing or Suggested Additions

- Add one sentence separating "Trotter can win in finite compiled costs" from "Trotter has better asymptotic scaling". The paper argues the former much more strongly than the latter.

