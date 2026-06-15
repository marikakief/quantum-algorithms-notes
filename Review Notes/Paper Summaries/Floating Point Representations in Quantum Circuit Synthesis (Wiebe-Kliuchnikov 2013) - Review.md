# Review: Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013)

Source note: [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes]]

## Primary Sources Checked

- Wiebe and Kliuchnikov, "Floating point representations in quantum circuit synthesis", arXiv:1305.5528 / New Journal of Physics 15, 093041 (2013).

## Verdict

Major issues. The note captures the high-level floating-point/gearbox idea, but it misstates the small-angle success-probability scaling and overstates some empirical claims as proved.

## Line-Anchored Comments

- Lines 17-19: the motivation and qualitative advantage over Solovay-Kitaev are good.
- Lines 35-45: line 45 is wrong. Since `P = cos^4(theta) + sin^4(theta) = 1 - 2 sin^2(theta) cos^2(theta) = 1 - (1/2) sin^2(2theta)`, for small `theta` this is `1 - 2 theta^2 + O(theta^4)`, not `1 - O(theta^4)`.
- Lines 47-65: the recursive squaring/floating-point construction is explained well, but the analogy should be kept informal; this is angle-squaring plus mantissa synthesis, not general floating-point arithmetic.
- Lines 75-83: the expected T-count discussion is too schematic to be used as a theorem statement. If exact recurrences are not reproduced, label this as intuition and cite the paper's recurrence equations.
- Lines 85-87: "provably help" is too strong if the comparison is against empirically observed ancilla-free scaling in Fig. 9/Table II. Say the paper gives strong evidence or a demonstrated advantage under its model unless a formal lower bound is cited.
- Lines 106-113: the caveats are good and should remain.

## Missing or Suggested Additions

- Add a warning that RUS circuits optimize expected cost, but resource estimates also need tail bounds or retry-budget policy.

