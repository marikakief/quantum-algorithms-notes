# Review: Interaction Picture Simulation (Kinetic Frame)

Source note: [[Interaction Picture Simulation (Kinetic Frame)]]

## Primary Sources Checked

- Low and Wiebe, interaction-picture Hamiltonian simulation, arXiv:1805.00675.
- Babbush et al., arXiv:1807.09802, first-quantized chemistry application.

## Verdict

Major issues. The Dyson-series framework is described well, but the source attribution and the norm-dominance arithmetic around the kinetic frame need correction.

## Line-Anchored Comments

- Line 1: the source line is confused. The interaction-picture technique is Low-Wiebe, arXiv:1805.00675. The Obsidian link points to Childs-Ostrander-Su randomization, which is a different 2019 product-formula/randomization paper.
- Lines 6 and 40: the final first-quantized chemistry gate scaling is right at the level of the Babbush et al. 2019 asymptotic result.
- Lines 10 and 30: the general message that `||A||` enters only logarithmically is good, but the condition should be phrased using `A` being efficiently simulable and `B` having small LCU norm, not just `||A|| >> lambda_B`.
- Lines 20-28: good compact summary of Low-Wiebe query/gate complexity.
- Lines 36-38: the displayed ratio `||T||/lambda_{U+V} = O(N^{1/3}/eta^2)` implies kinetic dominance only when `N >> eta^6`, not merely `N >> eta`. This matters because the note sells the physical intuition too strongly.
- Lines 43: the contrast with Low-Wiebe's second-quantized split is useful, but "physics is reversed" should be kept as intuition, not a formal dominance claim without parameter conditions.
- Lines 51: recomputing `sum_j ||k_pj||^2` is the 2019 asymptotic implementation. The 2021 follow-up introduced the incremental-register optimization; the card mentions this later, but the main text should say the compiled cost improved.
- Line 81: "does not directly reduce query complexity for phase estimation" is a bit muddy. Interaction-picture simulation reduces the effective norm paid for the simulated part; phase estimation still has Heisenberg scaling in the relevant normalization/precision.

## Missing or Suggested Additions

- Fix the source link and then add a two-column comparison: abstract Low-Wiebe interaction picture versus Babbush et al. first-quantized kinetic-frame application.

