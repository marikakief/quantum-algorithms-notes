# Review: Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025)

Source note: [[Quantum Algorithms for Representation-Theoretic Multiplicities (Larocca-Havlicek 2025) — Paper Notes]]

## Primary Sources Checked

- Larocca and Havlicek, "Quantum Algorithms for Representation-Theoretic Multiplicities," arXiv:2407.17649v5. I checked the arXiv version history: v5 was posted March 25, 2025 and explicitly incorporates the Panova correction.
- Panova, "Polynomial time classical versus quantum algorithms for representation theoretic multiplicities," arXiv:2502.20253v2.

## Verdict

Minor issues. This is a careful note on a moving target. It correctly updates the speedup story after Panova: the Fourier-sampling algorithm is clean, but the original superpolynomial Kronecker advantage claim is gone.

## Line-Anchored Comments

- Lines 11-19: good computational template. The dimension-ratio parameter is the right complexity bottleneck.
- Lines 23-28: the table is useful. In the plethysm row, keep both conditions: polynomial dimension ratio and an efficient QFT over the relevant wreath product subgroup.
- Lines 34-36: the assessment matches the current arXiv status. The paper's v5 abstract says Panova disproved the Kronecker conjecture and leaves a polynomial-gap framing.
- Lines 48-80: the restriction algorithm is well explained. The state `rho_G^alpha` is maximally mixed on the isotypic block, so the preparation cost/state model should be treated as part of the theorem, not free background.
- Lines 84-114: the four instantiations are clear. The Kronecker low-dimensional family is now a polynomial-gap comparison, not evidence for superpolynomial quantum advantage.
- Lines 124-140: the induction-side algorithm is a good addition. The HSP connection at line 140 is correctly limited; this is multiplicity estimation, not solving an HSP instance.
- Lines 144-151: notation issue. "Let `H` denote the gate size of the `H`-QFT" overloads subgroup `H`; rename this parameter in the note, e.g. `C_H`, for readability.
- Lines 161-165: the integer-rounding sample bound is right, but it gives constant success. Mention repetition/median if a high-confidence exact answer is needed.
- Lines 197-209: caveats are strong. The "state preparation hides work" point should remain in any copy of the theorem.

## Missing or Suggested Additions

- Add an explicit version/status line: "reviewed against arXiv v5, after Panova."
- Add a small table separating established classical collapses, conjectural classical regimes, and remaining plausible quantum windows.

